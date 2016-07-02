# Kubernetes on Bare Metal 
## What is Kubernetes? 
Kubernetes is an open source cluster management system developed by Google. It automates and simplifies a significant portion of managing containerized applications running on a clustered environments. Kubernetes or k8s is custer independent, it can be run on popular cloud providers (AWS, Google Cloud) or on bare metal. Running kubernetes on Google Cloud is extreamly easy since google manages k8s for you. I have been experimenting with k8s on Google Container Engine but in order to better understand the architechture I decided to deploy kubernetes on bare metal. 

## Kubernetes Master Architecture 
A kubernetes cluster consists of a master server and a number of worker nodes. The master is the externally accessible component that allows the execution of arbituatry commands on the dumber worker nodes. The following are the components that run on the master.

### Etcd 
Etcd is an open source distributed key-value store developed by the CoreOS team. Kubernetes uses `etcd` to store configuration data that can be accessed by all nodes in the cluster. The configuration data relates to service discovery and the state of the cluster that is used during configuration or reconfiguration of the nodes. 

One of the main draws of `etcd` is that the store is replicated across distributed systems but it is possible for the user to turn off `etcd` in all kubernetes server except the master. In such a scenario, all the worker nodes point to the master instance of the store. This is acceptable since the master Snode needs to always be available for kubernetes to function.

### API Server 
In order to make the cluster externally accessible, the master node runs an API server. This is the entry point for management of the entire cluster. It’s also responsible for ensuring that the `etcd` store and the details of the deployed containers are in agreement. The API server implements a RESTful interface to make communication simple and easily accessed.

### Controller Manager 
The controller manager is responsible for handling other cluster level functions such as replication. When the user states a desired state for the cluster, the controller manager service writes that state to `etcd` and the controller manager reads this information and impements the replication procedure in order to meet the desired state. 

### Scheduler Server 
The sceduler is responsible to assign work to the nodes in the cluster. It’s responsible for binding unscheduled pods to the nodes based on its requirements and the available infrastructure. This server is also responsible for monitoring resources across the nodes and ensure pods/works is only enqueued if there are enough resources available.

## Kubernetes Worker Architecture 
Every kubernetes server, master or worker has one basic requirement, Docker. Docker is responsible for pulling and running applications/images. In order for kubernetes to work, each worker must have access to a dedicated subnet. In order to accomplish this, a networking layer called `flannel` (by CoreOS) can be used. Docker configuration needs to be edited in order to use `flannel` and expose the required ports. Flannel only needs to be present on the master node.

### Kubelet 
The kubelet service is responsible for managing the the pods and their containers, images, volumes, etc. It does this by communicating with the master and listening to the `etcd` store for configuration changes.

### Kube Proxy 
Each node in the cluster runs a simple network proxy and a load balancer. This proxy does primitive load balancing and routes requests to the correct container running the node. 

## Kubernetes on Ubuntu 
In order to deploy kubernetes on an Ubuntu based cluster I relied heavily on using docker. Instead of installing `etcd` and `flannel` directly on the host OS, they are being run in docker containers on the host. The required master and worker components are also being run in a containerized environment. This allows the installation to be platform agnostic and makes it easy to setup. Instead of writing my own script to deploy kubernetes, I instead relied community script available on Github. In order to make setting up the server even easier, I wrote an Ansible role available on Ansible Galaxy. 

Deploying kubernetes on bare metal forced me to understand the underlying architechture regarding the communication between the master and worker nodes. Google Container Engineer does a great job of making kubernetes easily accessibly by abstracting away the underlying complexity and through a managed kubernetes master node. 

##Future Plans 
With the ansible role I was able to deploy and run a kubernetes cluster on bare metal. The next steps for me are deploying an load balancer with an external public IP in order to make kubernetes services extrenally accissible through a single IP. I will update the ansible role once I have a functioning load balancer. Feel free to reach out on twitter or github. Cheers
