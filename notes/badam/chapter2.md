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



