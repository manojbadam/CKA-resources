## Chapter 3
This chapter is about installation and configuration

* We will be using kubeadm for installing the cluster, it is still in beta. Another way of installing cluster - kubespray & Kops

* We need to configure what pod network we are going to use. We can use flannel for simplicity, but it doesnt support network security policies. Refer [here](https://github.com/coreos/flannel#networking-details) for more details.

* Instead of downloading individual kubernetes binaries, there is a container image called `hyperkube`, which contains all the key kubernetes binaries. So that we can start kubernetes cluster with few container images.

* Kubeadm installation contains mainly two steps 
    ** Running `kubeadm init` command in master node
    ** Running `kubeadm join` command in worker nodes

### Kubectl
* To configure cluster, use `kubectl` or `REST API` or `golang client`
* Default config path for `kubectl` is `~/.kube/config` 
* Kubectl contains cluster definitions (api server endpoints etc..) , context, credentials. 
* Context is combination of cluster and user credentials
* Context can be used to swtich between multiple clusters. 

* Minikube starts a single binary called localkube, which runs all the components of kubernetes together.

### Kubeadm
* Helps to bootstrap minimum cluster with best practises
* It also supports other cluster lifecycle functions like upgrades, downgrade. 
* It can used alongside with terraform or ansible during bootstrapping.
##### Kubeadm installation
* Kubernetes releases are generally supported for 9 months. 
* If you join a node with a different architecture to your cluster, create a separate Deployment or DaemonSet for `kube-proxy` and `kube-dns` on the node. This is because the Docker images for these components do not currently support multi-architecture.
* `kubeadm-config` is stored a configMap in kube-system namespace.
* `cluster-info` is stored as configMap in kube-public namespace.
* Once `kubeadm init` bootstrapping is finished, it will display the `kubeadm join` command. It includes a secret, needs to be stored securly.
*  `kubeadm init` prints that it installed addons like `coreDNS` and `kube-proxy`. We didnt have the pod networking add-on installed before that. And `CoreDNS` will not start up before pod network is installed.
* `kubeadm` only support CNI based network addons not `kubenet` based
* Beware that pod network must not overlap with any of host networks as this can cause issues. (Pod cidr and VPC cidr shouldn't overlap)
* 

#### TODO List
* In our EKS cluster, we are using the `hyperkube` image, verify how are we using it ?
* Can we use kubectl contexts in place of `kethos` switches ? 
* Also can we add some flags to `make eks-kubeconfig` to name the context (to switch between clusters), also default namespace for that. 
* [Stretch] Prepare a simple golang app which simulates the controller, scheduler, kubelet etc.. 
* Read about [Kubernetes conformance tests](https://kubernetes.io/blog/2017/10/software-conformance-certification/)
* Read about Static pods in kubelet 
* Try out the kubeadm installation in digital ocean and document the steps. 
* Read about kubenet network addon

#### Questions
* So 
* 