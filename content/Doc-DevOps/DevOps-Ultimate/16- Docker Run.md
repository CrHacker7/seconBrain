
to run another version of redis

> [!warning] ***docker run redis:4.0***
> - To run another version of redis.
> - By default, docker will consider the latest tag.

## RUN - STDIN
- We have a simple prompt app that when run asks for my name. And on entering my name, it will print a welcome message, if I wereee to dockerize this app and run it as docker container like this, it wouldn't wait for the prompt. It just prints whatever the app is supposed to print on standard out. ***IT RUNS IN A NON-INTERACTIVE MODE.***
By default docker container does not listen to a standard input, even though if we are attached to the console, it is not able to read any input from us. It does not have a terminal to read inputs from us.
- If we want to provide an input, we must map the standard input of our host to the docker container using the ***-i*** parameter.

> [!warning] ***docker run -i kodekloud/simple-prompt-docker***
> - The dash i parameter is for ***interactive*** mode and when I input my name, it prints the expected output.
- But there is still something missing from this, the prompt. At first, it asks us for our name, but when dockerize, that prompt is missing, even though it seems to have accepted my input.
- That's because the app prompt on the terminal and we have not attached to the containers terminal.

> [!warning] ***docker run -it kodekloud/simple-prompt-docker***
> - The dash t parameter stands for a pseudo terminal.
> - So the combination of both are now attached to the terminal as well as in an interactive mode on the container.

## RUN - PORT mapping
- There is 2 options to connect to a web browser:
1. It is to use the IP of the docker container, every docker container gets an IP assigned by default. It is an internal IP and only accessible within the docker host.

> [!warning] ***docker run kodekloud/webapp***
> http.//0.0.0.0:5000/
> http.//172.17.0.2:5000/
> Users outside of the docker host cannot access it using this IP
> - For this we could use the IP of the docker host, which is 192.168.1.5
> - For that to work, we must have mapped the port inside the docker container to a free port on the docker host

> [!tip] Exemple
> ***docker run -p 80:5000 kodekloud/webapp***
> - So the user can access my app by going to the URL Http://192.168.1.5:80
> - All traffic on port 80 on my docker host will get routed to port 5000 inside the docker container.
> - This way you can run multiple instances of your app and map them to different ports on the dock

## RUN - VOLUME MAPPING
When databases and tables are created, the data files are stored in location /var/lib/mysql
- If we want to persist data, we would want to map a directory outside the container on the docker host to a directory inside the container.

> [!warning] ***docker run -v /opt/datadir:/var/lib/mysql mysql***
> We create a directory called ***/opt/datadir*** and map to ***/var/lib/mysql*** inside the docker container with the ***-v*** option and specifying the directory on the docker host followed by a colon ***:*** and the directory inside the docker container.
> - It will implicity mount the external directory to a folder inside the docker container. All the data will be stored in the external volume at /opt/datadir and thus will remain even if we delete the docker container.

## INSPECT CONTAINER

> [!warning] ***docker inspect blissful_hopper***
> - It returns all details of a container in a JSON format, such as the state mounts, configuration data, network settings, etc.

## CONTAINER LOGS
How do I view the logs, which happens to be the contents written to the standard out of that container?

> [!warning] ***docker logs blissful_hopper***

## DEMO
> [!warning] ***docker run ubuntu sleep 15***
It will get stuck in the console during 15 seconds and won't go out even with CTRL + C while it is alive won't respond, then after 15s it will exit. 

##### **Two ways to access the web**
1. Using the internal IP 
	1. docker run jenkins
	2. docker inspect 3d4d
	3. On the browser (172.168.10.2:8080) 
2. By mapping a port to the docker host and accessing it using the external IP.
	1. docker run -p 8080:8080 jenkins
	2. On the browser (192.168.1.14:8080) 


> [!warning] ***docker inspect e2fke3***
> It shows the status, the image, networks and ***IPAddress*** 
> - It is the internal IP access. 
> - We need to be inside the docker host.
> - We can get the password for the admin user.

Externally, the port or service is not listening on the docker host, so we need to add a port mapping.
> [!danger] Attention!
> We cannot add a port mapping while the container is running.
##### VOLUME
If we want to persist the data even if we destroy de container, or stop, or restart, we have to map a volume.

> [!warning] ***docker run -p 8080:8080 -v /root/my-jenkins-data:/var/jenkins_home  -u root jenkins***
> 1. mkdir my-jenkins-data
> 2. docker run -p 8080:8080 -v /root/my-jenkins-data:/var/jenkins_home  -u root jenkins

LAB
***How many ports are published on this container?***
- 2 (ipv4-ipv6= 1 port)
```java
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS         PORTS                                                                                NAMES
64ae241a5356   nginx:alpine   "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   0.0.0.0:3456->3456/tcp, :::3456->3456/tcp, 0.0.0.0:38080->80/tcp, :::38080->80/tcp   quirky_brattain
```
Each mapping is usually in the format host_port->container_port.
Each -> indicates a published port. Count these to determine how many ports are published.

`0.0.0.0:3456->3456/tcp, :::3456->3456/tcp, 0.0.0.0:38080->80/tcp, :::38080->80/tcp`
- They are represented in different formats (IPv4 and IPv6). This means there is actually only one unique port published.

***Which of the below ports are the exposed on the CONTAINER?***
- 3456 & 80
***Which of the below ports are published on Host?***
- The ports on the left side of the -> symbol are the ones published on the host.
- 38080 & 3456
***Run an instance of kodekloud/simple-webapp with a tag blue and map port 8080 on the container to 38282 on the host.***
- In Docker, when you map ports between the host and the container, the format is typically ==***host_port:container_port.***==
- Container Port (8080): This is the port inside the container that the application is listening on. In your task, the kodekloud/simple-webapp container is set to listen on port 8080.
- Host Port (38282): This is the port on your host machine that is mapped to the container's port. When you access the application from your host, you use this port.
So, when you see 38282:8080, it means that port 38282 on your host is mapped to port 8080 inside the container.


HACER PULL-REQUEST A DEVELOP, NUNCA A MASTER, eso si es una feature.
feature -> release -> master
- es mejor ssh porque por http si cambias tu password tienes que cambiarlo en todos los repositorios 

.ssh config

resolv.conf

artefact: build test deploy securtity, quality test, deploy = .war es la imagen de docker

nexus