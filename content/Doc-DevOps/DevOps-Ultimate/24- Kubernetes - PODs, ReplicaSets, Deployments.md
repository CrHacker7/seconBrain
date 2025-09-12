K8s uses YAML files as inputs for the creation of objects such as pods, replicas, deployments, services, etc.
#### YAML In Kubernetes
- A k8s definition file always contains four top level fields.

> [!warning] pod-definition.yml
```java
apiVersion: //string
kind:       //string
metadata:   

spec:      //dictionary
```
1. is the version of the k8s API we are using to create the obj.
2. The kind refers to the type of obj we are trying to create.
3. Metadata is data about the obj, like name, labels, etc. Everything under metadata is intended to the right a bit. The name (child of metadata) is a string value, and labels is a dictionary within the metadata dictionary, and it can have any key-value as we wish.
4. Spec is a dictionary, so add a property under it called **containers**.
	1. Containers is a list/array because the *ports* can have multiple containers within them

| Kind       | Version |
| ---------- | ------- |
| POD        | v1      |
| Service    | v1      |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 |

> [!warning] Be careful with the format
> Check the doc for not misconfigure the metadata and spec.

> [!tip] pod-definition.yml
```java
apiVersion: v1  //string
kind: Pod      //string
metadata:
	name: myapp-pd         //     ---\
	labels:  //dictionary          ---|---- //dictionary
		app: myapp         //      ---|
		type: front-end    //     ---/
spec:
	containers:   //List/Array
		- name: nginx-container  //dash's 1st item of the list
		  image: nginx //docker image in docker repository
```

1. Running the ***kubectl create -f pod-definition.yml***
2. List the pods ***kubectl get pods***
3. We can check Info, when was created, labels, what docker containers are part of it and the events associated with that pod  ***kubectl describe pod myap-pod***

**Run the command: You can check for apiVersion of replicaset by command kubectl api-resources | grep replicaset**

> [!warning] create & apply 
> are the same in the first time

```java
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    tier: frontend                                                                                                      spec:
  containers:
  - name: nginx
    image: nginx
  - name: busybox
    image: busybox 
```
---


> [!NOTE] Pod-definition
```java
apiVersion: v1
kind: Pod
metadata:
  name: postgres
  labels:
    tier: db-tier
spec:
  containers:
    - name: postgres
      image: postgres
      env:
        - name: POSTGRES_PASSWORD
          value: mysecretpassword
```
## LAB [lab](lab k8s)
***How many pods exist on the system? In the current(default) namespace.***
- **kubectl get pods**
- **kubectl get pods -o wide** 
	- we have a new column of NODE habilitated
***Create a new pod with the nginx image.***
- **kubectl run nginx --image=nginx**
***What is the image used to create the new pods?
You must look at one of the new pods in detail to figure this out.***
- **kubectl describe pod \<correct-pod-name>**
 ***What is the state of the container agentx in the pod webapp?
Wait for it to finish the ContainerCreating state***
```java
 Container ID:   
    Image:          agentx
    Image ID:       
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
```
***What does the READY column in the output of the kubectl get pods command indicate?***
```java
NAME            READY   STATUS             RESTARTS      AGE
newpods-8cc2n   1/1     Running            1 (12m ago)   29m
newpods-jf6kn   1/1     Running            1 (12m ago)   29m
newpods-zq6kr   1/1     Running            1 (12m ago)   29m
nginx           1/1     Running            0             23m
webapp          1/2     ImagePullBackOff   0             5m50s
```
- running containers in POD/total containers in POD
***Delete the webapp Pod.
Once deleted, wait for the pod to fully terminate.***
- kubectl delete pod webapp

***Create a new pod with the name redis and the image redis123.
Use a pod-definition YAML file. And yes the image name is wrong!***
- ***kubectl run redis --image=redis123 --dry-run=client -o yaml D redis-definition.yaml***

> [!warning] kubectl run redis --image=redis123 --dry-run=client -o yaml D redis-definition.yaml
> WIth the D shows me the contents

```java
controlplane ~ ➜  kubectl run redis --image=redis123 --dry-run=client -o yaml D redis-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: redis
  name: redis
spec:
  containers:
  - args:
    - D
    - redis-definition.yaml
    image: redis123
    name: redis
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

**kubectl create -f redis-definition.yaml
pod/redis created**
```java
controlplane ~ ➜  kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-def
inition.yaml

controlplane ~ ➜  ls
redis-definition.yaml  sample.yaml

controlplane ~ ➜  kubectl create -f redis-definition.yaml
pod/redis created
```
***Now change the image on this pod to redis.
Once done, the pod should be in a running state.***
- Update the pod-definition file and use kubectl apply command or use :  **kubectl edit pod redis**
- If you used a pod definition file then update the image from redis123 to redis in the definition file via Vi or Nano editor and then run kubectl apply command to update the image :-
- **kubectl apply -f redis-definition.yaml** 
	- *I didn't apply it because i used the **kubectl get pod redis -o yaml > redis-definition.yaml***


> [!attention] kubectl create -f | kubectl apply
> kubectl **create -f** : create resources from a file.
>  kubectl **apply**: used for **updating existing** resources or **creating** them if they don't exist.

- kubectl edit modifies the pod's configuration in the cluster.
- Local files like redis-definition.yaml remain unchanged unless you manually update them.
To ensure your local file reflects the current state of the pod, you can export the pod's configuration back to a YAML file using:
> [!ATTENTION] ***kubectl edit pod redis ***
> Should ensure the local files reflects the current state of the POD.
***Vim: Warning: Output is not to a terminal***
> IF ERROR use **get** instead of **edit**

> [!warning] ***kubectl get pod redis -o yaml > redis-definition.yaml***
> to ensure local files reflects the current state of the pod

***Create a new pod with the name redis and the image redis123.
Use a pod-definition YAML file. And yes the image name is wrong!
1. Create a Pod Definition YAML File:
- You can use the kubectl run command to generate a YAML file for the pod. This command will create a YAML file without actually creating the pod:
==**kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml**==
This command will create a file named redis-definition.yaml with the pod definition.
2. Create the Pod from the YAML File:
- Once you have the YAML file, you can create the pod using the following command:
**kubectl create -f redis-definition.yaml**
3. Verify the Pod Creation:
- To ensure that the pod has been created, you can list all pods with:
**kubectl get pods**


> [!tip] edit replicaset
> ***kubectl edit rs new-replica-set***
> ***kubectl get pods --show-labels*** -> verificamos si ha cambiado la image
> 
> ***kubectl delete pods -l name=busybox-pod***-> elimina los pods de la imagen


> [!summary] Terminal
> **controlplane ~ ➜**  kubectl edit rs new-replica-set
replicaset.apps/new-replica-set edited
**controlplane ~ ➜**  kubectl get pods --show-labels
NAME                    READY   STATUS             RESTARTS   AGE     LABELS
new-replica-set-85rj8   0/1     ImagePullBackOff   0          7m14s   name=busybox-pod
new-replica-set-qvtsq   0/1     ImagePullBackOff   0          6m58s   name=busybox-pod
new-replica-set-rmtqn   0/1     ImagePullBackOff   0          7m5s    name=busybox-pod
new-replica-set-t6q24   0/1     ImagePullBackOff   0          7m25s   name=busybox-pod
**controlplane ~ ➜**  kubectl delete pods -l name=busybox-pod
pod "new-replica-set-85rj8" deleted
pod "new-replica-set-qvtsq" deleted
pod "new-replica-set-rmtqn" deleted
pod "new-replica-set-t6q24" deleted

> [!todo] Modificar las replicas
> - ***kubectl edit replicaset new-replica-set***
> - ***kubectl edit rs new-replica-set***
> - ***kubectl scale replicaset***
> - ***kubectl scale rs new-replica-set --replicas=5*** -> to scale up to 5 PODs.

***
 You can **manually** create the YAML file for the pod. Here's a basic example of what the redis-definition.yaml file might look like:
```java
 apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis123
```
1. Create the YAML File:
Use a text editor to create a file named redis-definition.yaml and paste the above content into it.

2. Create the Pod from the YAML File:
Once you've saved the file, you can create the pod using:
**kubectl create -f redis-definition.yaml**
3. Verify the Pod Creation:
**kubectl get pods**
##### Differences between these 2  ways
**Using kubectl run with --dry-run:**
Advantages:
- Quick and Easy: It's a fast way to generate a basic YAML file without manually writing it.
- Consistency: Ensures that the generated YAML is syntactically correct and adheres to Kubernetes standards.
- Good for Beginners: Helps those new to Kubernetes get started quickly without worrying about YAML syntax.

Disadvantages:
- Limited Customization: The generated YAML might not include all the fields you might want to customize, so you may need to edit it afterward.

**Manually Creating the YAML File:**
Advantages:
- Full Control: You can customize every aspect of the pod's configuration, adding any fields or settings you need.
- Learning Opportunity: Writing YAML manually helps you understand the structure and options available in Kubernetes configurations.

Disadvantages:
- Error-Prone: There's a higher chance of making syntax errors, especially for those new to YAML or Kubernetes.
- Time-Consuming: It can be slower, especially for complex configurations.

**Which is Better?**
- For Simple Tasks: Using kubectl run with ***--dry-run*** is often quicker and easier, especially for straightforward pod configurations.
- For Complex Configurations: Manually writing the YAML is better when you need to customize the pod extensively or include additional Kubernetes resources.
## Replication Controller

**Why we need a replication controller?**
If our application crashes, we do not have access to it, so we prevent users from losing access with the replica controller. 
RC helps us run multiple instances of a single pot in the k8s cluster, even if we have a single POD the RC can helpl by automatically bringing up a new POD.
Another reason, it is to create multiple ports to share the load across them. When the number of users increase, we deploy additional POD to balance the load across the 2 PODS, if we were to run out of resources on the first node, we can deploy additional parts across the other nodes in the cluster.
There are 2 similar terms:
1. Replication Controller: is the older technology replaced by Replica Set
2. Replica Set: is the new recommended way to set up replication.

> [!NOTE] rc-definition.yml (Replication Controller)
```java
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp.rc
  labels:
      app: myapp
      type: front-end //tier
spec: 
  template: // another metadata from a pod yaml   
      metadata:
        name: myapp-pod
        labels:
            app: myapp
            type: front-end
    spec:
      containers:
      -  name: nginx-container
         image: nginx // end metadata
 replicas: 3 // siblings with template

```
> [!tip] kubectl create -f rc-definition.yml
> creating command

> [!warning] kubectl get replicationcontroller
> to view the list of created replication controllers

> [!success] kubectl get pods
> to view the current numbers of replicas (pods)


> [!NOTE] replicaset-definition.yml (Replica Set)
```java
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp.replicaset
  labels:
      app: myapp
      type: front-end //tier
spec: 
  template: // another metadata from a pod
  
      metadata:
        name: myapp-pod
        labels:
            app: myapp
            type: front-end
    spec:
      containers:
      -  name: nginx-container
         image: nginx // end metadata
 replicas: 3 // siblings with template
 selector:  // THE DIFFERENCE WITH RC
     matchLables:
         type: front-end
       

```
Selector helps the replica set identify what parts fall under it. It can also manage parts that were not created as part of the replica set creation.
For example: there were parts created before the creation of the replica set that match labels specified in the selector, the replica set will also take those parts into consideration when creating the replicas.
The matchLabels selector simply matches the labels specified under it to the labels on the pod.
> [!tip] kubectl create -f replicaset-definition.yml
> creating command

> [!warning] kubectl get replicationset
> to view the list of created replication controllers

> [!success] kubectl get pods
> to view the current numbers of replicas (pods)


**Why do we labels our pods and objects in k8s?**
We can monitor existing parts if we have them already created, in case they were not created, the replica set will create them for us. Its role is to monitor the pods, if any of them were to fail, deploy new ones.
**how does the replica set know what parts to monitor?**
This is where labeling our parts during creation comes in handy. We could provide these labels as a filter for replica set, this way the replica set knows which parts to monitor.
The template section provides to the replica set what has to be created in case it crashes
**How do we update to scale to 6 replicas?**
1. Update the definition file. And run the `kubectl replace -f replicaset-definition.yml`
2. `kubectl scale --replicas=6 -f replicaset-definition.yml` or we can provide the type-name format `kubectl scale --replicas=6 replicaset myapp-replicaset` The `-f xxx.yml` command will not be updated in the file.

**Commands**
`kubectl create -f replicaset-definition.yml`
`kubectl get replicaset`
`kubectl delete replicaset myapp-replicaset`(also deletes all underlying POD's)
`kubectl replace -f replicaset-definition.yml`
`kubectl scale --replicas=6 -f replicaset-definition.yml`

