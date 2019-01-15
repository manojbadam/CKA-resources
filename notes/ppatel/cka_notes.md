CKA Training Notes
======

* [CKA Training Notes](#cka-training-notes)
*   * [Chapter 1 - Introduction](#chapter-1---introduction)
*     * [Chapter 2 - Basics of Kubernetes](#chapter-2---basics-of-kubernetes)
*         * [K8s architecture](#k8s-architecture)
*             * [Terminology](#terminology)
*                   * [Controllers:](#controllers)
*                         * [Other:](#other)
*                             * [Tools:](#tools)
*                               * [Chapter 3 - Installation and Configuration](#chapter-3---installation-and-configuration)
*                                   * [Installers](#installers)
*                                       * [kubectl](#kubectl)
*                                           * [pod networking](#pod-networking)
*                                               * [Installing components](#installing-components)
*                                                   * [Compiling from source](#compiling-from-source)
## Chapter 1 - Introduction
* intro to course layout, not much new content
* * [firwalld](https://firewalld.org/) is a replacement iptables
## Chapter 2 - Basics of Kubernetes
* k8s came from google project [borg](https://ai.google/research/pubs/pub43438)
* ### K8s architecture
* ![alt text](https://lms.quickstart.com/custom/858487/images/Kubernetes%20Architecture.png "k8s architecture")
* control (aka master)
*   * api server
  * various controllers
  *   * storage system to hold state of cluster container settings & networking configurations
  *   * exposes an API (can use kubectl _or_ curl)
  *   * **kube-scheduler** : forwarded requests for running containers coming to the API, finds a node to run the container
  *   * nodes run **kubelet** & **kube-proxy** (cilium replaces kube-proxy?)
  *   * kubelet 
  *     * receives requests to run the containers/manages resources/monitors
  *       * interacts with container engine, usually docker, can be rkt or cri-o
  * creates/manages network rules to expose container on network

* **pod** - contains one or more containers, share an ip address access to storage and namespace
* **controllers** - watch-loops/manage orchestration
*   * ask kube-apiserver for a particular object state
*     * modify the object until declared state matches the current state
*     * **kube-controller-manager** - controllers compiled into
*     * **Deployment** - default controller
*       * ensures resources declared in the PodSpec are avail (ex. ip address, storage)
*         * deploys a **ReplicaSet**
*         * **ReplicaSet** - controller that deploys/restarts pods
*         * **Jobs** / **CronJobs** : one-off/recurring tasks
#### Other:
* **labels** - arbitrary strings, become part of object metadata, used to make managing x000's of pods easier
* * **taints** / **toleration** - used to discourage pod assignments [see more here](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)
* *  **annotations** - remain w/ object, can't be used by k8s commands - used by 3rd party agents/other tools
### Tools:

* **minikube** - used inside virtualbox
* * **kompose** - translate docker compose files into k8s objects [kubernetes/kompose](https://github.com/kubernetes/kompose)
## Chapter 3 - Installation and Configuration
### Installers
  * [kubeadm guide](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/)
  *   * [kubespray](https://github.com/kubernetes-sigs/kubespray)
  *     * [kops](https://github.com/kubernetes/kops)
  *       * [hyperkube](https://github.com/kubernetes/kubernetes/tree/master/cluster/images/hyperkube)
  *           * contains all the k8s binaries (kubelet etc)
* `kubectl config use-context foobar`
*   * **context** switches the "current" config/cluster/credentials
*   ### pod networking
*   * [kubernetes networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
*     * kubernetes model:
*         * all containers can communicate with all other containers without NAT
*             * all nodes can communicate with all containers (and vice-versa) without NAT
*                 * the IP that a container sees itself as is the same IP that others see it as
*                 * cni helps adhere to that contract 
*                 * [cncf CNI](https://www.cncf.io/blog/2017/05/23/cncf-hosts-container-networking-interface-cni/)
*                   * lots of networking choices (calico, flannel, kube-router, romana, weave net)
*                   ### Installing components
*                   * use systemd to run kubelet/master components
*                   * api-server has various flags to enable/disable features
*                   * can run these components individually, or inside a container themselves with [hyperkube](https://console.cloud.google.com/gcr/images/google-containers/GLOBAL/hyperkube)
*                   ### Compiling from source
cd $GOPATH
git clone https://github.com/kubernetes/kubernetes.git
cd kubernetes
make
```


