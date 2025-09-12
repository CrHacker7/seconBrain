Our aim(objetivo)
#### POD
- It is the smallest object that we can create in k8s.
```java
   +------------------------------------------------------+   
   |                 Kubernetes Cluster                   |
   |  +-----------------------+  +---------------------+  | 
   |  |         Node          |  |          Node       |  | 
   |  |     +-----------+     |  |     +-----------+   |  |  
   |  |     |    POD    |     |  |     |    POD    |   |  |   
   |  |     | +-------+ |     |  |     | +-------+ |   |  |   
   |  |     | |  App  | |     |  |     | |  App  | |   |  |   
   |  |     | +-------+ |     |  |     | +-------+ |   |  |   
   |  |     |           |     |  |     |           |   |  |   
   |  |     +-----------+     |  |     +-----------+   |  | 
   |  |                       |  |                     |  |
   |  +-----------------------+  +---------------------+  | 
   +------------------------------------------------------+ 
```
We can have helpers for every app container. it maintains 1 to 1 relationship and thus needs to communicate with the app containers directly and access data from those containers.
- For this, we need to maintain a map of what app and helper containers are connected to each other.
- We need to establish network connectivity between these containers ourselves using links and custom networks.
- We need to create shareable volumes and share it among the containers.
- We need to monitor the state of the app container and when it dies manually kill the helper container as well. When a new container is deployed, we'd need to deploy the new helper container as well.

| App     | Helper | Volume |
| ------- | ------ | ------ |
| Python1 | App1   | Vol1   |
| Python2 | App2   | Vol2   |
|

```java
+-----------------------------------------------------------+
|                        NODE                               |
| +------------+ +-----------+ +------------+ +-----------+ |
| | +--------+ | | +--------+| | +--------+ | | +--------+| |
| | | python | | | | python || | | python | | | | python || |  
| | +--------+ | | +--------+| | +--------+ | | +--------+| |
| | +--------+ | | +--------+| | +--------+ | | +--------+| |
| | | helper | | | | helper || | | helper | | | | helper || |  
| | +--------+ | | +--------+| | +--------+ | | +--------+| |
| |    POD     | |    POD    | |     POD    | |     POD   | |
| +-----------+ +------------+ +------------+ +-----------+ |
+-----------------------------------------------------------+
```
Multi-pod containers are a rare use case
#### How to deploy a POD?
1. Create a POD automatically and deploys an instance of the nginx docker image. We need to specify the image name using the image parameter
	1. `kubectl run nginx --image nginx`
2. It helps us see the list of parts in our cluster
	1. `kubectl get pods`
3. We can have all the info
	1. `kubectl describe pod nginx`
4. This provides addtional information such as the node where the pod is running and the IP of the pod as well. Each POD gets an internal IP of its own within the k8s cluster
	1. `kubetctl get pods .o wide`

A note about creating pods using ***kubectl run***.

To create a pod from the command line, use the command:

###### Create an NGINX Pod
`kubectl run nginx --image=nginx`

As of version 1.18, kubectl run (without any arguments such as --generator ) will create a pod instead of a deployment.

- To create a deployment using imperative command, use kubectl create:
	***kubectl create deployment nginx --image=nginx***