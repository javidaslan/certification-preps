# Certified Kubernetes Administrator (CKA)

This guide aims to help in your CKA exam preparation using courses, simulators and other online materials. 

## Intro

The Certified Kubernetes Administrator (CKA) program provides assurance that CKAs have the skills, knowledge, and competency to perform the responsibilities of Kubernetes administrators.

- The exams are delivered online and consist of performance-based tasks (problems) to be solved on the command line running Linux.
- The exams consist of 15-20 performance-based tasks.
- Candidates have 2 hours to complete the CKA and CKAD exam.
- The exams are proctored remotely via streaming audio, video, and screen sharing feeds.

Results will be emailed 24 hours from the time that the exam is completed.

It is important to highlight that, CKA Exam is an open book exam. This means, you are allowed to use [Official Kubernetes Documentation](https://kubernetes.io/docs/home/) during the exam.

### Pass Score

A score of 66% or above must be earned to pass the exam.

## Exam Curriculum

This exam focuses on testing of your knowledge in following areas:

### Storage (10%)

Understand [storage](https://kubernetes.io/docs/concepts/storage/) concepts: 

- [storage classes](https://kubernetes.io/docs/concepts/storage/storage-classes/), [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [volume mode](https://kubernetes.io/docs/concepts/storage/persistent-volumes/), [access modes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) and [reclaim policies](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) for volumes
- [persistent volume claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) primitive
- How to [configure applications with persistent storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

### Troubleshooting (30%)

- [Evaluate cluster and node logging](https://kubernetes.io/docs/tasks/debug/)
- [Manage container stdout & stderr logs](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [Understand how to monitor applications](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/)
- [Troubleshoot application failure](https://kubernetes.io/docs/tasks/debug/debug-application/)
- [Troubleshoot cluster component failure](https://kubernetes.io/docs/tasks/debug/debug-cluster/)
- Troubleshoot networking:
    - [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
    - [Debug Services](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/)

### Workloads & Scheduling (15%)

- Understand deployments and how to perform rolling update and rollbacks
    - [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
    - [Performing Rolling Update](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)
- Use ConfigMaps and Secrets to configure applications
    - [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
    - [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
    - [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
    - [Uses for Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#uses-for-secrets)
- Know how to scale applications
- Understand the primitives used to create robust, self-healing, application deployments
    - [Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
    - [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- Understand how resource limits can affect Pod scheduling
    - [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
    - [Configure Default Memory Requests and Limits for a Namespace](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)
- Awareness of manifest management and common templating tools
    - [Declarative Management of Kubernetes Objects Using Configuration Files](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)
    - [Managing Kubernetes Objects Using Imperative Commands](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/)
    - [JSONPath Support](https://kubernetes.io/docs/reference/kubectl/jsonpath/)

### Cluster Architecture, Installation & Configuration (25%)

- Manage role based access control (RBAC)
    - [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
    - [Service Accounts](https://kubernetes.io/docs/concepts/security/service-accounts/)
    - [Authenticating](https://kubernetes.io/docs/reference/access-authn-authz/authentication/)
- Use Kubeadm to install a basic cluster
    - [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
    - [Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
- Manage a highly-available Kubernetes cluster
    - [Creating Highly Available Clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/)
- Provision underlying infrastructure to deploy a Kubernetes cluster
- Perform a version upgrade on a Kubernetes cluster using Kubeadm
    - [Upgrading kubeadm clusters](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
- Implement etcd backup and restore
    - [Operating etcd clusters for Kubernetes](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/)

### Services & Networking (20%)

- Understand host networking configuration on the cluster nodes
    - [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
- Understand connectivity between Pods
    - [Pod Networking](https://kubernetes.io/docs/concepts/workloads/pods/#pod-networking)
- Understand ClusterIP, NodePort, LoadBalancer service types and endpoints
    - [Service](https://kubernetes.io/docs/concepts/services-networking/service)
    - [Service Types](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)
- Know how to use Ingress controllers and Ingress resources
    - [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- Know how to configure and use CoreDNS
    - [CoreDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns)
- Choose an appropriate container network interface plugin
    - [Network Plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)

## Preparation Guides

As in most of certification exams guide is simple:

- Take the course
- Practise

### Rule of Thumb

Before going into details of what course to take or where to practice, as a general rule for this exam, you should have the following skills that could save you a lot of time during the exam:

- Learn how to properly use [Kubernetes Documentation](https://kubernetes.io/docs/home/). Searching in this page is quite helpful, try to find necessary resource quickly.
- Learn how to use `kubectl` help page.
- Learn [imperative way of creating/modifying objects](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/). You can see more examples in [cheatsheet](./cheatsheet.md).
- You will have [vim](https://www.vim.org/about.php) at your disposal as a text editor to create/modify K8S manifests. Learn following basic directives to operate withing vim easily:

    ```
    # insert mode "a"
    # exit mode "ESC"
    # set line numbers: ":set nu"
    # set autoindent: ":set autoindent"
    # quit without saving ":q!"
    # save and quit ":wq"
    # search in fole: "?<search word>"
    # go to line n: ":<line_number>"
    # go to the beginning of file: "gg"
    # go to the end of line: "shift+4"
    # go to the end of file: "shift+G"
    # move cursor to right by word: "w"
    # move cursor to left by word: "b"
    # undo the last change: "u"
    # enter visual block mode: "ctrl+v"
    # enter block mode: "V"
    # copy: "y"
    # cut: "c"
    # paste: "p"
    ```

### Courses

- [Certified Kubernetes Administrator (CKA) with Practice Tests](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/?couponCode=KEEPLEARNING)
- [Certified Kubernetes Administrator](https://www.pluralsight.com/cloud-guru/courses/certified-kubernetes-administrator-cka)

### Practise

- [CKA Simulator](https://killer.sh/cka)
- [Ultimate Certified Kubernetes Administrator (CKA) Mock Exam Series](https://kodekloud.com/courses/ultimate-certified-kubernetes-administrator-cka-mock-exam/)

### During the exam

Spend ~5 mins to setup your environment during the exam:

- Set your aliases:
    - `k` is already aliased to `kubectl` for you. So you don't have to do it.
    - set alias for [`dry-run`](https://kubernetes.io/docs/reference/kubectl/conventions/#best-practices:~:text=You%20can%20use%20the%20%2D%2Ddry%2Drun%3Dclient%20flag%20to%20preview%20the%20object%20that%20would%20be%20sent%20to%20your%20cluster%2C%20without%20really%20submitting%20it): `alias do=--dry-run=clien`.
    - customize your vim setup. I personally added the following directives to `~/.vimrc`:
        - `set nu` show line numbers
        - Additionally check [Vim documentation: options](https://vimdoc.sourceforge.net/htmldoc/options.html)

### Cheatsheet

Additionally, I prepared a small [cheatsheet](./cheatsheet.md) that's comprised of commonly used imperative commands, steps to configure certain components and etc.