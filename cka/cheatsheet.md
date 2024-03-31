# Cheatsheet

This cheatsheet consists of imperative commands, steps to implement a certain scenario and etc.

## Disclaimer

This cheatsheet does not cover all the possible commands you may need in exam. Here you can find only most frequent ones and the scenarios that are slightly complex to remember.

## During the exam

Spend ~5 mins to setup your environment during the exam:

- Set your aliases:
    - `k` is already aliased to `kubectl` for you. So you don't have to do it.
    - set alias for [`dry-run`](https://kubernetes.io/docs/reference/kubectl/conventions/#best-practices:~:text=You%20can%20use%20the%20%2D%2Ddry%2Drun%3Dclient%20flag%20to%20preview%20the%20object%20that%20would%20be%20sent%20to%20your%20cluster%2C%20without%20really%20submitting%20it): `alias do=--dry-run=clien`.
    - customize your vim setup. I personally added the following directives to `~/.vimrc`:
        - `set nu` show line numbers
        - Additionally check [Vim documentation: options](https://vimdoc.sourceforge.net/htmldoc/options.html)

## Static Pods

Manifests of static pods are stored in `/etc/kubernetes/manifests` location of node. Control plane objects such as etcd, kube-apiserver, kube-controller-manager and kube-scheduler have their manifests stored in this location on master node.

## kubectl

It is very important to know how to use `kubectl --help`. This will accelerate your speed at exam, because during the exam you're connected to remote desktop and therefore there is lag while scrolling in documentation. So going to documentation and coming back to your terminal can be bothersome.

For example, here is how to use help page to get more information on how to create pod via imperative command:

```
 ~$> kubectl create deployment --help                                                                                     
Create a deployment with the specified name.

Aliases:
deployment, deploy

Examples:
  # Create a deployment named my-dep that runs the busybox image
  kubectl create deployment my-dep --image=busybox

  # Create a deployment with a command
  kubectl create deployment my-dep --image=busybox -- date

  # Create a deployment named my-dep that runs the nginx image with 3 replicas
  kubectl create deployment my-dep --image=nginx --replicas=3

  # Create a deployment named my-dep that runs the busybox image and expose port 5701
  kubectl create deployment my-dep --image=busybox --port=5701

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
the template. Only applies to golang and jsonpath output formats.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be
sent, without sending it. If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
      --image=[]: Image names to run.
  -o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --port=-1: The port that this container exposes.
  -r, --replicas=1: Number of replicas to create. Default is 1.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --show-managed-fields=false: If true, keep the managedFields when printing objects in JSON or YAML format.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create deployment NAME --image=image -- [COMMAND] [args...] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```

## Cluster wide commands

```
# show config
kubectl config view

# show cluster info
kubectl cluster-info --kubeconfig <config_file|defaults to ~/.kube/config>

# show contexts
kubectl config get-contexts

# switch context
kubectl config use-context <context_name>

# switch namespace
kubectl config set-context --current --namespace <namespace>
```

## Imperative commands

### Workloads

```
# create pod
kubectl run <pod_name> --image <image>

# Example: generate pod YAML file (-o yaml) without creating pod (--dry-run=client) [dry-run]
kubectl run nginx --image nginx:latest --dry-run=client -o yaml > pod.yaml

# create a deployment
kubectl create deployment <deployment_name> --image=<image> 

# Example: generate deployment YAML file (-o yaml) without creating it (--dry-run)
kubectl create deployment --image=<nginx> nginx --dry-run=client -o yaml > deployment.yaml

# scale deployment
kubectl scale deployment <deployment_name> --replicas=<number_of_replicas>

# update image in deployment
kubectl set image deployment <deployment> <image>

# rollouts
Rollouts can give information about the changes in revisions of your deployment.

# get the rollout status of deployment
kubectl rollout status deployment/<deployment_name>

# get the revision history of deployment
kubectl rollout history deployment/<deployment_name>

# rollback deployment/frontend to previous revision
kubectl rollout undo deployment/<deployment_name>
```

### Service

```
# Expose nginx pod on port 80 via port 30080 on the nodes:
# This means service type of NodePort must be created. There are 2 ways of doing it.

# First: his will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod:

kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml > svc.yaml

# Second: This will not use the pods labels as selectors
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml > svc.yaml (or you can directly create it without dry-rung)

# Expose deployment via service
kubectl expose deployment <deployment> --port <portnumber> --target-port <portnumber>
```

### App config

```
# create config map
kubectl create cm <name> --from-literal key1=value1 --from-literal key2=value2
kubectl create cm <name> --from-file filename

# create secret
kubectl create secret <name> --from-literal key1=value1 --from-literal key2=value2
kubectl create secret <name> --from-file filename
```

## Security

### create CertificateSigningRequest in K8s (CSR)

```
# Step 1: encode .csr file:
cat <file>.csr | base64 -w 0

# Step 2: create CSR manifest:
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
    - system:authenticated
  request: <encoded text from step 1 goes here>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth


# Step 3: Apply 
k apply -f <csr.yaml>

# list CertificateSigningRequest in K8s (CSR)
kubectl get csr

# approve/deny certificate signing request
kubectl certificate approve/deny <csr_name>
```

### RBAC

```
# check if you have access to certain apis, e.g.:
kubectl auth can-i create deployments

# as an admin, you can impersonate other users:
kubectl auth can-i create deployments --as <username>

# you can specify namespace too
kubectl auth can-i create deployments --as <username> --namespace <namespace>

# create role & rolebinding
kubectl create role foo --verb=get,list,watch --resource=rs.apps --resource-name=readablepod --resource-name=anotherpod
kubectl create rolebinding admin --clusterrole=admin (or --role=<rolename>) --user=user1 --user=user2 --group=group1

# create token for service account
kubectl create serviceaccount <serviceaccount_name>
```

## Cluster config & maintenance

### kubeadm

#### Provision K8S cluster with kubeadm

[https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

1. Deploy your (virtual) nodes
2. Install runtime (on all nodes)
3. install kubeadm, kubelet and kubectl (on all nodes)
4. create cluster with kubeadm:
    <br> Only on master node:
    - initialize cluster: `sudo kubeadm init --pod-network-cidr=<ip/mask> --apiserver-advertise-address=<ip address of API server>` 
    - deploy pod network: [https://kubernetes.io/docs/concepts/cluster-administration/addons/](https://kubernetes.io/docs/concepts/cluster-administration/addons/)
    <br> Worker node
    - join worker node to cluster: `kubeadm join <apiserver IP> --token=<> --discovery-token-ca-cert-hash<>` 

#### Cluster Upgrades via kubeadm

##### Master node

1. Plan the upgrade: `kubeadm upgrade plan`. In this phase you might be asked to upgrade kubeadm tool first. For this use `apt get upgrade -y kubeadm=<version>` command.
2. Apply: `kubeadm upgrade apply <version>`
3. Upgrade `kubelet`: `apt get upgrade -y kubelet=<version>`
4. Restart kubelet service: `systemctl restart kubelet`
5. For the other control plane nodes: `sudo kubeadm upgrade node`

##### Worker node

1. Drain node first: `kubectl drain <nodeName>`
2. Plan the upgrade: `kubeadm upgrade plan`
3. Apply: `kubeadm upgrade apply <version>`
4. Upgrade `kubelet`: `apt get upgrade -y kubelet=<version>`
5. Restart kubelet service: `systemctl restart kubelet`
5. Uncordon node: `kubectl uncordon <nodeName>`

### etcd

You can work with `etcdctl` on master node. But before issuing any command make sure you're using the v3 of `etcdctl API`. For this first set the version:

`export ETCDCTL_API=3`

Let's see how to backup and restore etcd:

#### Backup

If our ETCD database is TLS-Enabled, the following options are mandatory:

- `--cacert` verify certificates of TLS-enabled secure servers using this CA bundle
- `--cert `  identify secure client using this TLS certificate file
- `--endpoints=[127.0.0.1:2379]` This is the default as ETCD is running on master node and exposed on localhost 2379.
- `--key` identify secure client using this TLS key file

To find all the above it is enough to look at etcd yaml manifest stored in `/etc/kubernetes/manifests/etcd.yaml`.

Finally, command to backup etcd will be as following:

```
etcdctl \
  --endpoints=<endpoints|defaults to 127.0.0.1:2379> \
  --cacert=<cacert_file> \
  --cert=<cert_file> \
  --key=<key_file> \
  snapshot save <filename>
```

#### Restore:
```
# Restore file to data directory:
etcdctl \
  --endpoints=<endpoints|defaults to 127.0.0.1:2379> \
  --cacert=<cacert_file> \
  --cert=<cert_file> \
  --key=<key_file> \
  snapshot restore <filename> --data-dir=<data_directory>

# Update /etc/kubernetes/manifests/etcd.yaml and set etcd directory to data_directory and wait for the etcd pod to be restarted. This may take some time.
```

## Networking

### CNI

- all plugins located under: `/opt/cni/bin`
- see configured plugin under: `ls /etc/cni/net.d/`

### Ingress

```
# create ingress in imperative way
kubectl create ingress <name> --rule="<rule>" --dry-run=client -o yaml > ing-pay.yaml
```


## Troubleshooting

### Kubelet troubleshooting

```
# check kubelet process state
systemctl status kubelet.service

# check kubelet logs
journalctl -u kubelet -f
```

kubelet config locations:
  1. /var/lib/kubelet/config.yaml
  2. /etc/kubernetes/kubelet.conf.

## Using jsonpath

There will be some questions where you must get the certain information from pods and store it into file. In some cases, it requires to use jsonpath.

[jsonpath.com](https://jsonpath.com/) is good playground.


Also, let's see some examples on how to use it in actual cluster:

```
# get name and IP address of nginx pod
kubectl get pod nginx -o=jsonpath='{.metadata.name}  {.status.podIP}'

# get names of nodes in loop using range:
kubectl get nodes -o=jsonpath='{range $.items[*]}{$.metadata.name}{"\n"}{end}'
```