## Chapter 2

Projects born out of Borg 
1. Mesos
2. Kubernetes
3. Omega
4. Cloud Foundry

Projects born out of Cgroups
1. Docker
    a. rkt/appC
    b. OCI
2. LxC


* Orchestration happens through series of watch loops or controllers. 
* Each controller interacts with api server for particular object state. It tries to modify the object until it comes to declared state.
* These controllers are compiled into Kube-controller-manager. 
* Replication Controller is moved to Deployments. 

* Kubeadm - cluster spin up
* helm, kompose - configure cluster
* k8s came from google project [borg](https://ai.google/research/pubs/pub43438)

### K8s architecture
![alt text](https://lms.quickstart.com/custom/858487/images/Kubernetes%20Architecture.png "k8s architecture")

* control (aka master)
  * api server
  * scheduler
  * various controllers
  * storage system to hold state of cluster container settings & networking configurations
* exposes an API (can use kubectl _or_ curl)
* **kube-scheduler** : forwarded requests for running containers coming to the API, finds a node to run the container
* nodes run **kubelet** & **kube-proxy** (cilium replaces kube-proxy?)
* kubelet 
  * receives requests to run the containers/manages resources/monitors
  * interacts with container engine, usually docker, can be rkt or cri-o
* kube-proxy
  * creates/manages network rules to expose container on network
### Terminology

* **pod** - contains one or more containers, share an ip address access to storage and namespace
#### Controllers:
* **controllers** - watch-loops/manage orchestration
  * ask kube-apiserver for a particular object state
  * modify the object until declared state matches the current state
* **kube-controller-manager** - controllers compiled into
* **Deployment** - default controller
  * ensures resources declared in the PodSpec are avail (ex. ip address, storage)
  * deploys a **ReplicaSet**
* **ReplicaSet** - controller that deploys/restarts pods
* **Jobs** / **CronJobs** : one-off/recurring tasks

#### Other:
* **labels** - arbitrary strings, become part of object metadata, used to make managing x000's of pods easier
* **taints** / **toleration** - used to discourage pod assignments [see more here](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)
*  **annotations** - remain w/ object, can't be used by k8s commands - used by 3rd party agents/other tools

### Tools:

* **minikube** - used inside virtualbox
* **kompose** - translate docker compose files into k8s objects [kubernetes/kompose](https://github.com/kubernetes/kompose)