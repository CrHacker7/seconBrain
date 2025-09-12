##### The introduction of the course
1. Containers - Docker
2. Container Orchestration
3. Demo - Setup Kubernetes
4. Kubernetes Concepts - PODs | ReplicaSets | Deployment | Services
5. Networking in Kubernetes
6. Kubernetes Management - Kubectl
7. Kubernetes Definition Files - YAML
8. Kubernetes on Cloud - AWS/GCP
The most popular container orchestration Technology.

Container + Orchestration
##### Why do you need containers?
1. Compatibility/Dependency
2. Long setup time
3. Different Dev/Test/Prod environments
- With docker, we were able to run each component in a separate container with its own libraries and its own dependencies, all in the same VM and OS, but within separate environments or containers.
##### Containerize Applications
- Run each service with its own deps in separate containers 
```java
    Container     Container      Container     Container 
   +-----------+ +-----------+ +-----------+ +------------+
   | WebServer | | Database  | | Messaging | |Orchestration
   |  NODEJS   | |  MONGODB  | |  REDDIS   | |  ANSIBLE   |
   |  EXPRESS  | |  COUCHDB  | |           | |            |  
   |----+ +----| |----+ +----| |----+ +----| |----+ +-----|
   |Libs| |Deps| |Libs| |Deps| |Libs| |Deps| |Libs| |Deps |
   +-----------+ +-----------+ +-----------+ +------------+ 
   +------------------------------------------------------+   
   |                       Docker                         |
   +------------------------------------------------------+   
   +------------------------------------------------------+   
   |                         OS                           |
   +------------------------------------------------------+   
   +------------------------------------------------------+   
   |                 Hardware Infrastructure              |
   +------------------------------------------------------+   
```
##### What are containers?
- Containers are completely isolated environments, as in they can have their own processes or services, their own networking interfaces, their own mounts, just like VM. Except they're all shared the same OS Kernel.
##### Operating Sytem
Like ubuntu, fedora, suzy, centOS. 
- They all consist of two things: 
1. OS Kernel:
	1. It is responsible for interacting with the hardware. And it is the same for all of them, which is LINUX.
2. Set of Software:
	1. It may consist of a different user interface drivers, compilers, file managers, developer tool, etc.
So we won't able to run a windows based container on a docker host with Linux OS on it. For this would require docker on a Windows server.
##### Virtual Machines
```java
        VM             VM           VM            VM
   +-----------+ +-----------+ +-----------+ +------------+
   |Application| |Application| |Application| |Application |
   |-----------| |-----------| |-----------| |------------|
   |----+ +----| |----+ +----| |----+ +----+ |----+ +-----|
   |Libs| |Deps| |Libs| |Deps| |Libs| |Deps| |Libs| |Deps |
   |-----------| |-----------| |-----------| |------------| 
   |-----------| |-----------| |-----------| |------------|
   |    OS     | |    OS     | |    OS     | |    OS      |
   +-----------+ +-----------+ +-----------+ +------------+ 
   +------------------------------------------------------+   
   |                       Hypervisor                     |
   +------------------------------------------------------+ 
   +------------------------------------------------------+   
   |                       Docker                         |
   +------------------------------------------------------+   
   +------------------------------------------------------+   
   |                 Hardware Infrastructure              |
   +------------------------------------------------------+ 
utilization: HIGH | size: GB | bout up: A LOT OF TIME (SLOW)
```
##### Containers
```java
      Container    Container     Container     Container
   +-----------+ +-----------+ +-----------+ +------------+
   |Application| |Application| |Application| |Application |
   |-----------| |-----------| |-----------| |------------|
   |----+ +----| |----+ +----| |----+ +----+ |----+ +-----|
   |Libs| |Deps| |Libs| |Deps| |Libs| |Deps| |Libs| |Deps |
   +-----------+ +-----------+ +-----------+ +------------+ 
   +------------------------------------------------------+   
   |                       Docker                         |
   +------------------------------------------------------+  
   +------------------------------------------------------+   
   |                         OS                           |
   +------------------------------------------------------+ 
   +------------------------------------------------------+   
   |                 Hardware Infrastructure              |
   +------------------------------------------------------+ 
utilization: LOW | size: MB | Boot up: SHORT TIME (FASTER)
```
##### How is it done?
If we run many containers of the same image, many instances, we need to configure the LOAD BALANCER in the front, in case one of the instances was to fail, simply destroy that instance and launch a new one
##### Container vs Image
- An image is a package or a template. It is used to create containers.
- Containers are running instances of the images that are isolated and have their own environments and set of processes.
##### Container Orchestration
- The platform needs to orchestrate the conectivity between the containers and automatically scale up or down based on the load.
#### Kubernetes Architecture
##### Nodes (Minios)
- It is a machine, physical or virtual, on which k8s is installed.
- It a worker machine, and is where containers will be launched by k8s
- We need to have more than one node if one fails.
##### Cluster
- it's a set of nodes grouped together
- If one node fails, we have the app sitll accessible from the others. 
- Having multiples nodes helps in sharing load.
##### Master
- Responsible for managing the cluster (orchestration), It monitores the nodes.
- It is another node and is configured as a master
- It stores the info about the members of the cluster.
- When a node fails, it moves the workload of the failed node to another worker node.
##### Components
1. API Server
	1. Acts as a frontend, the users, management devices, CLI, all talk to the API Server to interact with the k8s cluster.
2. etcd
	1. Distributed reliable key-value. it stores all data from nodes and masters of the cluster. It's responsible to implement locks within the cluster to ensure that there're no conflicts between the masters 
3. Kubelet
	1. Is the agent that runs on each node in the cluster. Responsible for making sure that the cotianers are running n the nodes as expected.
4. Container Runtime (Docker, rkt= rocket, CRI-O)
	1. Is the underlying software that is used to run containers, in our case, docker.
5. Controller
	1. Are the brain behind the orchestration. Responsible for noticing when nodes, containers or endpoints goes down. So they make decisions to bring up new containers in such cases.
6. Scheduler
	1. Is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to nodes.
##### Master vs Worker Nodes
- Master has the ***kube-apiserver*** that is what makes it a master.
- Worker nodes have the **kubelet*** agent that is responsible for interacting with the master to provide health info of the worker node and carry out actions requested by the master.
- All the info gathered(recopilado) are stored in a key-value store (etcd) on the master
- Master has the controller and the scheduler.
```JAVA
   +------------------------+   +-------------------------+   
   |   </>kube-apiserver <--------->   </>kubelet         |
   |          -etcd         |   |                         |  
   |       -controller      |   |                         |   
   |        -scheduler      |   |    -Container Runtime   |
   |                        |   |                         |
   |          MASTER        |   |      WORKER NODE        |
   +------------------------+   +-------------------------+
```
##### kubectl
- is used to deploy and manage app on a k8s cluster to get cluster info, to get the status of other nodes in the cluster.
-  `kubectl run hello-minikube` used to deploy an app on the cluster.
- `kubectl cluster-info` cmd is used to view info about the cluster.
- `kubectl get nodes` is used to list all the nodes part of the cluster.
