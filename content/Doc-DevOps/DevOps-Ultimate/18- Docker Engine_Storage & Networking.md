Docker Engine: It is referred to a host with docker installed on it.
```groovy
+-----------------------------------+
|          VIRTUAL MACHINE          |
|  +-----------------------------+  |
|  |        Docker Engine        |  |
|  |   +---------------------+   |  |
|  |   |      Docker CLI     |   |  |
|  |   +---------------------+   |  |
|  |   +---------------------+   |  |
|  |   |       REST API      |   |  |
|  |   +---------------------+   |  |
|  |   +---------------------+   |  |
|  |   |    Docker Deamon    |   |  |
|  |   +---------------------+   |  |
|  +-----------------------------+  |
+-----------------------------------+
```

DOCKER DAEMON: 
- Is a background process that manages docker objects such as the images, containers, volumes and networks
DOCKER REST API SERVER:
- Is the API Interface that programs can use to talk to the daemon and provide intructions. You can create your own tools using this REST API.
DOCKER CLI
- Is the Cmd Line Interface to perform actions such as running a container, stopping, destroy, etc. It uses the REST API to interact with Daemon
- It need not necessarily be on the same host

```java
+-------------------+      +-----------------+
|   DOCKER ENGINE   |      |     LAPTOP      |
|  +-------------+  |      |   +---------+   |
|  |  REST API   |-----------> Docker CLI|   |
|  +------|------+  |      |   +---------+   |
|  +------|------+  |      +-----------------+
|  |Docker Daemon|  |  
|  +-------------+  |  
+-------------------+
```

> [!warning] ***docker -H=remote-docker-engine:2375***

> [!warning] ***docker -H=10.123.2.1:2375 run nginx*** 

#### CONTAINERIZATION
Docker uses namespaces to isolate workspace, process IDs, network inter process, communication mounts.
- Namespaces: 
	1.  Process ID
	2. Network
	3. Unix Timesharing
	4. Mount
	5. InterProcess
- Namespace isolation technique:
	- Process ID: whenever a linux system builds up, it starts with just one process with a process ID of one. This is the root process ad kicks off all the other processes in the system.By the time the system boots up completely, we have a handful of processes running.
	- If we were to create a container, which is basically like a child's system within the current system, the child needs to think that is an independent system on its own and it has its own processes originating from a root process with a process ID of one. All PID are unique. There is not hard isolation between the containers and the underlying host.
	- So the processes running inside the container are in fact processes running on the underlying host. SO IT CANNOT HAVE TWO PIP OF ONE. There is where namespaces come into play with process ID namespaces.
```java

+--------------------------------------------------------+
|      LINUX SISTEM          +------------------------+  |   
|                            |CHILD SYSTEM (Container)|  |
|    +-------------+         |   +--------------+     |  |
|    |  PID: 1     |         |   | PID: 1       |     |  |
|    +-------------+         |   +--------------+     |  |
|    +----------+            |   +---------+          |  | 
|    | PID: 2   |            |   |  PID: 2 |          |  |
|    +----------+            |   +---------+          |  |
|    +----------+            +------------------------+  |
|    | PID: 2   |                                        |
|    +----------+                                        |
|    +----------+                                        |
|    | PID: 3   |                                        |
|    +----------+                                        |    
+--------------------------------------------------------+
```

The underlying docker host and the containers share the same system resources.
##### CGROUPS - CONTROL GROUPS
- To restrict the amount of hardware resources allocated to each container.
```java
+--------------------------------------------------------+
|                      LINUX SISTEM                      |
|    +-----------+    +-----------+    +-----------+     |   
|    | DOCKER    |    | DOCKER    |    |DOCKER     |     |
|    | CONTAINER |    | CONTAINER |    |CONTAINER  |     |
|    +-----------+    +-----------+    +-----------+     |  
|    +-------------------+     +-------------------+     |
|    |       CPU         |     |       MEMORY      |     |
|    +-------------------+     +-------------------+     |
+--------------------------------------------------------+


```
***docker run --cpus=.5 ubuntu***

> [!warning] ***docker run --cpus=.5 ubuntu***
>it ensures that the container does not take up more than 50% of the host CPu at any given time

> [!warning] ***docker run --memory=100m ubuntu***
> it can use until 100mb

## DOCKER STORAGE
##### FILE SYSTEM
- When we install docker on a system, it creates this folder structure:
	- ***/var/lib/docker***
		- inside we have: 
			- ***aufs***
			- ***containers***
			- ***image***
			- ***volumes, etc***
##### LAYERED ARCHITECTURE
- When we create an image it will be by layers, for every line of the dockerfile.
- It keeps in the caché when we reuse the image for a new container, so it makes it fast. The same image layer may be shared between multiple containers created from this image.
##### VOLUMES
- If we want to persiste our data from out container, i.e. in a database we would like to preserve the data created by the container.

> [!warning] ***docker volume create data_volume***
> it will create a folder within volumes folder.
> - /var/lib/docker
> 	- volumes
> 		- data_volume

-  using : 

> [!warning] ***docker run -v data_volume:/var/lib/mysql mysql***
> Then when I run the docker container using the docker run cmd, I could mount this volume inside the docker container READ/WRITE layer

There are two types of ***volume mounting*** and a ***bind Mount***
VOLUME MOUNT
- Mounts a volume from the volumes directory
BIND MOUNT
- Mounts a directory from any location on the docker host

```java
+----------------------------------------------------------+
|  +------------------------+ +------------------------+   |
|  |    READ WRITE          | |        READ WRITE      |   |
|  |  /var/lib/mysql        | |      /var/lib/mysql    |   |
|  |  mysql-container layer | | mysql-container layer  |   |
|  +------------------------+ +------------------------+   |
|                                                          |
|+-------------------------+   +------------------------+  | 
||      data_volume        |   |        mysql           |  |
|| /var/lib/docker/volumes |   |        /data           |  |
|+-------------------------+   +------------------------+  |
|                                                          | 
| +-----------------------------------------------------+  |
| |                   READ ONLY                         |  |
| |               mysql- image layer                    |  |
| +-----------------------------------------------------+  |
|                     DOCKER HOST                          |
+----------------------------------------------------------+
```
***docker run -v data_volume2:/var/lib/mysql mysql***
***docker run -v /data/mysql:/var/lib/mysql mysql***

--mount is the prefered way as it is more verbose, so we have to specify each parameter in a key equals value format.

***docker run 
--mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql***
- the type is bind
- the source is the location on my host
- the target is the location on my container
The responsible of doing all this, maintaining the layered architecture, creating a writable layer, moving files across layers to enable copy and write, etc. is the ***STORAGE DRIVERS***

The common storage drivers are:
- AUFS
- ZFS
- BTRFS
- Device Mapper
- Overlay
- Overlay2
The selection of the storagen driver depends on the OS, In ubuntu is AUFS, and this one is not available in other OS like Fedora or CentOS, In that case Device Mapper may be a better option.
- docker will choose the best storage driver automatically.
## LAB
***What location are the files related to the docker containers and images stored?***
- Location is /var/lib/docker.
***What directory under /var/lib/docker are the files related to the container alpine-3 image stored?***
- It's available in the location /var/lib/docker/containers/.
Run docker ps -a command and match the container id of alpine-3 with the directory name.
***Run a mysql container named mysql-db using the mysql image. Set database password to db_pass123
Note: Remember to run it in the detached mode.***
- *docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql*
***We have just written some data into the database. To view the information we wrote, run the get-data.sh script available in the /root directory. How many customers data have been written to the database?
Command: sh get-data.sh***

***Run a mysql container again, but this time map a volume to the container so that the data stored by the container is stored at /opt/data on the host.
Use the same name : mysql-db and same password: db_pass123 as before. Mysql stores data at /var/lib/mysql inside the container.***
- **docker run -v /opt/data:/var/lib/mysql -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql**
- we run **sh get-data.sh** and this will return all the db

## DOCKER NETWORKING
Default Networks: when we install it, creates 3 networks automatically.
> [!tip] ***Bridge***
> ***docker run ubuntu***
> - The default network a container gets attached to.
> - It is private internal network created by docker on the host, and they get an internal IP address range 172.17.0.0
> - To access any of these containers from the outside world,map the ports of these containers to ports on the docker host.
> - Another way to access externally is to associate the container to the host network

> [!tip] None
> ***docker run ubuntu --network=none***
> - The containers are not attached to any network and does not have any access to the external network or other containers. ISOLATE NETWORK


> [!tip] Host
> ***docker run ubuntu --network=host***
> - One way to access to the container from outside, it's to map the port 
> - Another way, it's to associate the container to the host network.
> - It deletes any isolation between the Host Docker and the Container.
> - It means we can run an app in a port 5000 without requireing any port mapping as the web container uses the host's network.
> - It also means that we will now ==not be able to run multiple web containers== on the same host and same port as the ports are now common to all containers in the host network.

***What is we wish to isolate the ocntainers ==within== the docker host?***
- By default, docker creates one internal bridge network.
- We can create our own internal networking and specify the driver, the subnet for that network, and the custom isolated network

> [!warning] ***docker network create --driver bridge --subnet 182.18.0.0/16 custom-isolated-network***

==***docker network ls***==
- we can list all networks
==***docker inspect blissful_hopper id/name***==
- list of network settings and IP address assigned to a container

## Embedded DNS
Containers can reach each other using their names

```java
       +------------+        +------------+
       |    Web     |        |  mysql     |
       |  container |        | container  |
       +------------+        +------------+
             |                    |
             |                    |
        172.17.0.2            172.17.0.3
            |                     |
             +--------+------------+
                      |
                 +-----------+
                 |  docker0  |
                 +-----------+
							                 +-----------+
							                 |DNS SERVER |
							                 +-----------+
                 mysql.connect(mysql)
```
***How can I get my web server to access the database on the database container?***
- To use the internal IP assigned to mysql container ==NOT GOOD IDEA== because it is not guaranteed that the container will get the same IP when the system reboots.
- The right way to do it is to use the container name.

> [!memo] 
> Docker has a built in DNS Server that helps the containers to resolve each other using the container name.

**DNS SERVER**

| Host  | IP         |
| ----- | ---------- |
| web   | 172.17.0.2 |
| mysql | 172.17.0.3 |
Docker uses network namespaces that creates a separate namespace for each container. It uses virtual ethernet pairs to connect containers together.

## LAB
***Explore the current setup and identify the number of networks that exist on this system.***
- docker network ls
***What is the ID associated with the bridge network?***
- docker inspect a2dcc
***We just ran a container named alpine-1. Identify the network it is attached to.***
- docker ps -a
- docker inspect alpine-1
```java
look for the line
"NetworkSettings": {
            "Bridge": "",
            "Ports": {},
            "HairpinMode": false,
            "Networks": {
                "host": {
            
-or the line  "NetworkMode": "host",            
```


***What is the subnet configured on bridge network?***
- `docker network inspect bridge`
```java
~ ✖ docker network inspect bridge | grep Subnet
                    "Subnet": "172.12.0.0/24",
```
***Run a container named alpine-2 using the alpine image and attach it to the none network.***
- docker run -d --name alpine-2 --network=none alpine
***Create a new network named wp-mysql-network using the bridge driver. Allocate subnet 182.18.0.0/24. Configure Gateway 182.18.0.1***
- **docker network create --driver bridge --subnet 182.18.0.0/24 --gateway 182.18.0.1 wp-mysql-network**
- docker network create --driver bridge --subnet=182.18.0.0/24 --gateway=182.18.0.1 wp-mysql-network

> [!warning] Both are okay 
> ***--subnet=182.18.0.0/24*** 
>  ***--subnet 182.18.0.0/24*** 


Inspect the created network by:
**docker network inspect wp-mysql-network**

***Deploy a mysql database using the mysql:5.6 image and name it mysql-db. Attach it to the newly created network wp-mysql-network
Set the database password to use db_pass123. The environment variable to set is MYSQL_ROOT_PASSWORD.***
- Not need to pull the image, run downloads it automatically.
- docker run -d -e MYSQL_ROOT_PASSWORD=db_pass123 --name mysql-db --network wp-mysql-network mysql:5.6
***Deploy a web application named webapp using the kodekloud/simple-webapp-mysql image. Expose the port to 38080 on the host.
The application makes use of two environment variable:
	1: DB_Host with the value mysql-db.
	2: DB_Password with the value db_pass123.
Make sure to attach it to the newly created network called wp-mysql-network.
Also make sure to link the MySQL and the webapp container.***
- `docker run --network=wp-mysql-network -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --name webapp --link mysql-db:mysql-db -d kodekloud/simple-webapp-mysql`
- ***docker run -d -p 38038:8080 -e DB_Host=mysql-db -e DB_Password=db_pass123 --network wp-mysql-network --name webapp --link mysql-db:mysql-db kodekloud/simple-webapp-mysql*** (my way)

> [!note] Be careful
> - For every ENV VAR, we need one -e
> - The --link option in Docker is used to allow containers to communicate with each other. In your current task, the --link mysql-db:mysql-db part of the command is ensuring that the webapp container can communicate with the mysql-db container using the alias mysql-db.
> - Here's a breakdown of what --link mysql-db:mysql-db does:
>

> [!tip] explaination
> mysql-db (first part): 
> - This is the name of the MySQL container you want to link to.
> 
>mysql-db (second part): 
> - This is the alias or "channel name" that the webapp container will use to refer to the MySQL container.

***
When we can not see the container running with docker *network ls* or *docker ps -a* but we are sure we have it and running it is because the ==container was run== with the ***--network host*** option, it won't appear in the list of networks because ==it's using the host's network stack== directly. This means ==it doesn't have its own network namespace==.

so here we have 2 containers:
--link mysql-db:mysql-db: This links the webapp container to the mysql-db container. The first mysql-db is the name of the MySQL container, and the second mysql-db is the alias used by the webapp container to communicate with the MySQL container.

#### WEBAPP (Frontend)
##### **Check Network Connection:**
 docker inspect webapp
Look for the Networks section in the output. Ensure that wp-mysql-network is listed there.

##### **Verify Communication:**
You can test the communication between the webapp and mysql-db containers by executing a command inside the webapp container to ping the mysql-db container:

 ***docker exec webapp ping -c 4 mysql-db***

If the ping is successful, it means the webapp container can communicate with the mysql-db container.
Check Environment Variables:

##### **Check Environment Variables:**

Ensure that the environment variables DB_Host and DB_Password are correctly set in the webapp container. You can verify this by inspecting the container:
bash
***docker inspect webapp | grep -A 5 "Env"***
Check that DB_Host=mysql-db and DB_Password=db_pass123 are present in the output.


#### MYSQL-DB (backend)
##### **Check Application Logs:**

Look at the logs of the webapp container to see if there are any specific error messages that can give you more insight:
 ***docker logs webapp***

##### **Database Connection:**

Ensure that the mysql-db container is running and accessible. You can check its status with:
 ***docker ps***

Verify that the MySQL service inside the mysql-db container is running properly. You might need to check the MySQL logs for any errors.
Database Credentials:

Double-check that the database credentials (DB_Host, DB_Password) are correctly set and match the MySQL configuration.
##### **Network Configuration:**

Ensure that both containers are on the same network (wp-mysql-network) and can communicate with each other, as you have already verified.
##### **Application Configuration:**

Make sure that the application inside the webapp container is configured to connect to the database using the correct host and credentials.
##### **Restart Containers:**

Sometimes, simply restarting the containers can resolve transient issues:
***docker restart mysql-db***
***docker restart webapp***