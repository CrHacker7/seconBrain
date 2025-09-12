##### What is the flavor and version of Operating System on which the Kubernetes nodes are running?
 - kubectl get nodes -o wide
 ##### How many containers are part of the pod webapp?
 - kubectl describe pod webapp
##### Create a new pod with the name *redis* and the image *redis123*
- kubectl run redis --image=redis123 --dry-run=client -o yaml
- kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml
- kubectl create -f redis.yaml
##### Edit the yaml file 
- cat redis.yaml
- vi redis.yaml
- kubectl apply -f redis.yaml
- kubectl get pods
##### Replication Controllers and ReplicaSets
- The replication controller  can help by automatically bringing up a new pod when the existing one fails. It ensures that the specified number of pods are running at all times
###### Replication Controller
- is the older technology that is being replaced by replica set
###### Replica Set
- New recommended way to set up replication
#### For creating the replica controller yaml 

> [!NOTE] rc-definition.yml
```java
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    tier: front-end 
spec:
  template:
    metadata: //from here add the metadata of a POD
      name: nginx  
      labels:
          app: myapp
          tier: front-end
    spec:
      containers:
	  - name: nginx-container
	    image: nginx
  replicas: 3
//FOR RUNNING IT 
Kubectl create -f rc-definition.yml
```
##### To view the list of created replication controllers
- kubectl get replicationcontroller
#### Replica Set
Its role is to monitor the pods and if any of them were to fail deploy new ones

> [!warning] replicaset-definition.yml
```java
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
	  app: myapp
      tier: front-end 
spec:
  template:
  
    metadata: //from here add the metadata of a POD
      name: nginx  
      labels:
          app: myapp
          tier: front-end
    spec:
      containers:
	  - name: nginx-container
	    image: nginx
  replicas: 3
  selector:
	  matchLabels:
		  tier: front-end
```
> [!failure] Requires a selector definition
- kubectl create -f relicaset-definition.yml
- kubectl get replicaset
#### Labels and Selectors
**why do we label our pods and objects in k8s?**
- Replica set knows which pods to monitor through labels and selectors.

> [!success] The values for 'labels' and 'template tier' should match!!
> We need to apply it: **kubectl apply -f replicaset-definition.yaml**
#### Scale the Replica Set
We started with 3 replicas but in the future we decided to scale to 6, for updating it.
there're severals ways to do it:
1. Update the number of replicas in the definition file to 6
-  Run **kubectl replace -f replicateset-definition.yml**
2. **kubectl scale --replicas=6 -f replicaset-definition.yml**
3. **kubectl scale --replicas=6 replicaset myapp-replicaset**
	- replicaset= type
	- myapp-replicaset = name
	- USING THE FILE NAME AS INPUT WILL NOT RESULT IN THE NUMBER OF REPLICAS BEING UPDATED AUTOMATICALLY ON THE FILE. SO, THE NUMBER OF REPLICAS IN THE DEFINITION WILL STILL BE 3
#### Summary
kubectl delete replicaset myapp-replicaset (aslo delete all underlying POD's)
- **kubectl delete replicaset \<replicaset-name> or** 
- **kubectl delete -f \<file-name>.yaml**
- **kubectl delete pods -l app=new-replica-set**
	- (delete all pods with label)

#### Edit the file created by k8s
- **kubectl edit replicaset myapp-replicaset**
- Then create it one more time for updating.
editing the object file configuration we can make permanet changes without manifiesto.
Another way is by comand line:
- kubectl scale --replicas=2 replicaset  myapp-replicaset

> [!NOTE] with this we can edit the config k8s' file
> kubectl get repilcaset
```java
controlplane ~ ➜  kubectl get replicaset
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       23m
```

> [!SUCCESS] ONE replica is a POD.

```java
controlplane ~ ✖ kubectl get pod
NAME                    READY   STATUS             RESTARTS   AGE
new-replica-set-ppnz8   0/1     ImagePullBackOff   0          4m53s
new-replica-set-q5lgj   0/1     ImagePullBackOff   0          4m53s
new-replica-set-qfnh9   0/1     ImagePullBackOff   0          4m53s
new-replica-set-vssnt   0/1     ImagePullBackOff   0          4m53s

controlplane ~ ✖ kubectl delete pod new-replica-set-ppnz8 
pod "new-replica-set-ppnz8" deleted

```
##### Scale the ReplicaSet to 5 PODs.
- kubectl scale rs new-replica-set --replicas=5 
- kubectl edit replicaset new-replica-set
##### Verify all the created things
`kubectl get all`

##### Add extension YAML in VSCODE
Install the extension YAML
CTRL + SHIFT + P = `palette de cmd`
User setting(JSON) 
User setting (all the configuration)
	extensions
	yaml -> schemas -> edit in settings.json
```yaml
	{
    "files.autoSave": "onFocusChange",
    "yaml.schemas": {
        
        "kubernetes": "*.yaml" //added

    }
}
```
	
#####
#####
#####
#####
#####