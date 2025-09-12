***When we need to create our own images?***
- We cannot find a component or a service that we want to use as part of our app on docker hub already.
- We decide to dockerize out app for ease of shipping and deployment.
***How to create my own image?***
 1. OS -Ubuntu
 2. Update apt repo
 3. Install dependencies using apt
 4. Install Python dependencies using pip
 5. Copy source code to / opt folder
 6. Run the web server using "flask" command.

> [!success] Dockerfile
> FROM Ubuntu
> RUN apt-get update
> RUN apt-get install python
> RUN pip install flask
> RUN pip install flask-my-sql
> COPY . /opt/source-code
> ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

> [!warning] ***docker build Dockerfile -t devops/my-custom-app***
> Create the image locally

> [!warning] ***docker push devops/my-custom-app***
> account name + image name
> Available in the docker hub.

###### INSTRUCTION + ARGUMENTS = Dockerfile
- All dockerfile must start with a ***FROM*** Instruction.
- ***RUN*** install all dependencies
- ***COPY*** copies files from local system onto the docker image. In our exemple, the source code is in out current folder (.) and I will be copying it to the location or source-code inside the docker image
- ***ENTRYPOINT*** allows us to specify a command that will be run when the image is run as a container

> [!warning] ***docker history devops/simple-webapp***
> We can see the history and size

We can containerized almost all of the app:
- browsers
- utilities like curl
- app like spotify, skype
Like this we not need to install anything, but using  docker

## DEMO
Installing an app from github <https://github.com/mmumshad/simple-webapp>

***docker run -it ubuntu bash***
> [!failure] apt-get install -y python
> we get the message: unable to locate package python. 
> - It is because we need to do ***apt-get update*** first.
> - Verify the installation: python

When we want to install flask with pip:
pip install flask 
	pip: command not found
apt-get install python-pip
	pip installed sucessfully
pip install flask
	installed sucessfully

##### Now, we need the source code of the app we want
	cat > /opt/app.py (create/copy app code to this dir)
	vi /opt/app.py
	apt-get install vim
then
	FLASK_APP=/opt/app.py flask run --host=0.0.0.0
We can see all the commands we just made with: ***history***
	it has to be inside the container

Dockerfile
1. mkdir my-simple-app
2. cd my-simple-app
3. cat > Dockerfile
	1. FROM ubuntu
	2. RUN apt-get update
	3. RUN apt-get install -y python python-pip (x2 dep)
	4. RUN pip install flask
	5. COPY app.py /opt/app.py (copy inside the docker cont.)
	6. ENTRYPOINT FLASK_APP=/opt/app.py flask run --host=0.0.0.0
4. cat > app.py
	1. go tho the gh and copy the source code (app.py)
5. docker build .

## LAB
***We just downloaded the code of an application. What is the base image used in the Dockerfile?
Inspect the Dockerfile in the webapp-color directory.***
- You can either open the file using vi /root/webapp-color/Dockerfile (or using commands such as cat/more/less/vim e.t.c) and look for the FROM instruction or search for it directly using grep -i FROM /root/webapp-color/Dockerfile.
- python:3.6
```python
~ ➜  ls
webapp-color
~ ➜  cd webapp-color 
webapp-color on  docker-images via 🐍 ➜  ls
Dockerfile        app.py            requirements.txt  templates
webapp-color on  docker-images via 🐍 ➜  cat Dockerfile 
FROM python:3.6
RUN pip install flask
COPY . /opt/
EXPOSE 8080
WORKDIR /opt
ENTRYPOINT ["python", "app.py"]
```
***To what location within the container is the application code copied to during a Docker build?
Inspect the Dockerfile in the webapp-color directory.***
- /opt
***Diferences between COPY - WORKDIR***
**COPY:** 
	The COPY instruction is used to copy files and directories from your host machine into the Docker image.
In your example, COPY . /opt/ copies everything from the current directory on your host machine to the /opt/ directory inside the Docker image.
**WORKDIR:**
	The WORKDIR instruction sets the working directory for any subsequent RUN, CMD, ENTRYPOINT, COPY, and ADD instructions in the Dockerfile.
In your example, WORKDIR /opt sets /opt as the working directory. This means any command that follows will be executed in the /opt directory inside the container.
In summary, COPY is about transferring files, while WORKDIR is about setting the default directory for executing commands.

***When a container is created using the image built with this Dockerfile, what is the command used to RUN the application inside it.
Inspect the Dockerfile in the webapp-color directory***
- Open the Dockerfile and look for ENTRYPOINT command.
- "python", "app.py"
***Build a docker image using the Dockerfile and name it webapp-color. No tag to be specified.***
- Move to the directory first by using the cd command and verify the path of the working directory from pwd command :-
```
$ cd /root/webapp-color/
$ pwd
/root/webapp-color
```
- $ ***docker build -t webapp-color .***
- NOTE: At the end of the command, we used the "." (dot) symbol which indicates for the current directory, so you need to run this command from within the directory that has the Dockerfile.
***Run an instance of the image webapp-color and publish port 8080 on the container to 8282 on the host.***
-  docker run -p 8282:8080 webapp-color
***What is the base Operating System used by the python:3.6 image?
If required, run an instance of the image to figure it out.***
- docker run python:3.6 cat /etc/*release*
```
cat: /etc/alpine-release: No such file or directory
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```
***the size of webapp-color?***
- 913MB
- **That's really BIG for a Docker Image. Docker images are supposed to be small and light weight. Let us try to trim it down.**
***Build a new smaller docker image by modifying the same Dockerfile and name it webapp-color and tag it lite.
Hint: Find a smaller base image for python:3.6. Make sure the final image is less than 150MB.***
- In the webapp-color directory, run the ls -l command to list the Dockerfile and other files.
- And modify Dockerfile to use ***python:3.6-alpine*** image and then build using ***docker build -t webapp-color:lite .***
```java
REPOSITORY                      TAG           IMAGE ID       CREATED          SIZE
webapp-color                    lite          18b3d67d323e   11 seconds ago   51.9MB
webapp-color                    latest        e290ddee044e   11 minutes ago   913MB
```
***Run an instance of the new image webapp-color:lite and publish port 8080 on the container to 8383 on the host.***
- docker run -d -p 8383:8080 webapp-color:lite
## Docker Environment Variable
If we want to change the background color of our app, it is better practice to do it with an env variable, like this we just need to run the container especyfing the color we want with the name of the variable and the name of the image
- To deploy multiple containers and set a different value for the env var.

> [!tips] ***docker run -e APP_COLOR=blue my-webapp-color***

> [!success] ***docker run -e APP_COLOR=green my-webapp-color***

> [!failure] ***docker run -e APP_COLOR=yellow my-webapp-color***


> [!warning] ***docker inspect blissful_hoppe***
> To inspect the properties of a running container.
> - Under the ***config*** section, we'll find the ***Env var*** set on the container

## LAB
***Inspect the environment variables set on the running container and identify the value set to the APP_COLOR variable.***
- Run this command to get the env fields from the inspect command: docker inspect \<container-name> | grep -A 10 Env
Replace container-name with the correct one.
	- grep: This is a command-line utility used for searching plain-text data for lines that match a regular expression.
	-A 10: This option tells grep to display 10 lines of context after each matching line. In this case, it shows the 10 lines following any line that contains "Env".
 specifically focusing on lines that contain the word "Env" and the 10 lines that follow it.
 
***Run a container named blue-app using image kodekloud/simple-webapp and set the environment variable APP_COLOR to blue. Make the application available on port 38282 on the host. The application listens on port 8080.***
- Run the command : docker run -p 38282:8080 --name blue-app -e APP_COLOR=blue -d kodekloud/simple-webapp

> [!failure] Order
> The order of the options and arguments in the docker run command is important. 

To know the env field from within a webapp container, run docker exec -it blue-app env

- when you placed **"kodekloud/simple-webapp --name blue-app -e APP_COLOR=blue"** after the image name, Docker ==treated these as arguments== to the command being ==run inside== the container, not as Docker options. By placing the image name at the end, you ensure that all Docker options are correctly applied before the container starts.
- Command line argument precedes over environment variable.
- --name (argument) ; -e APP_COLOR=blue (env var)

***Deploy a mysql database using the mysql image and name it mysql-db.
Set the database password to use db_pass123. Lookup the mysql image on Docker Hub and identify the correct environment variable to use for setting the root password.***
- docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
- docker exec -it mysql-db env  
```java
~ ➜  docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
d3213aa4aa9c2141090027d4c735e75b627d9186fc3d37038ccc6553c94ac7d1
~ ➜  docker exec -it mysql-db env          
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=d3213aa4aa9c
TERM=xterm
MYSQL_ROOT_PASSWORD=db_pass123
GOSU_VERSION=1.17
MYSQL_MAJOR=innovation
MYSQL_VERSION=9.0.1-1.el9
MYSQL_SHELL_VERSION=9.0.1-1.el9
HOME=/root
```

## CMD VS ENTRYPOINT

 Ubuntu image has ***CMD \["bash"]*** 
	 It is not really a process like a web server or database server. ***IT IS A SHELL THAT LISTENS FOR INPUTS FROM A TERMINAL***, if it cannot find a terminal, it EXITS.
By default, docker does not attach a terminal to a container when it is run, and so the bash program does not find the terminal and so it exits.

***So how do you specify a different cmd to start the container?***
1. Is to append a cmd to the docker run cmd, and that wau it overrides the default cmd specified within the image.

> [!tips] docker run ubuntu sleep 5

***How do I make this change permanent?***
```java
FROM Ubuntu
...
CMD sleep 5
```

> [!warning] Shell form
> -CMD command param1
> -CMD sleep 5

> [!warning] JSON array format
> The first element cmd should be executable
> - CMD \["command", "param1"]
> - CMD \["sleep", "5"]
> > [!failure] NOT specify the cmd param together
> > CMD \["sleep 5"] 


Now, we can build it and run it:
- ***docker build -t ubuntu-sleeper***
- ***docker run ubuntu-sleeper***
###### If we want to change the time, only need to write the number
> [!failure] docker run ubuntu-sleeper sleep 10

> [!success] docker run ubuntu-sleeper 10

`Command at Startup: sleep 10`

2. The ENTRYPOINT is like the cmd instruction, as you can specify the program that will be run when the container starts.
- CMD will override, and replaced entirely
- ENTRYPOINT the cmd line param will get appended
```java
FROM Ubuntu
...
ENTRYPOINT ["sleep"]
```

> [!success] ***docker run ubuntu-sleeper 10***

`Command at Startup: sleep 10`

***What if I run the ubuntu-sleeper image cmd without appending the number?***

> [!FAILURE] CMD without appending 
> It will get an error that the operand is missing

***How do you configure a default value for the cmd if one was not specified in the cmd line?***
- We would use both: CMD & ENTRYPOINT
- So the CMD instruction will be appended to the ENTRYPOINT

```java
 FROM ubuntu 
 ENTRYPOINT \["sleep"]
 CMD \["5"] 
```
> [!warning] It will start with sleep 5 if you didn't specify any param in the cmd line, if you did, then that will override the cmd instruction
> - Always in JSON format

> [!success] ***docker run --entrypoint sleep2.0 ubuntu-sleeper 10***
> - Command at Startup: sleep2.0 10 (imaginary cmd)
> - Like this we can modify the ENTRYPOINT during runtime

## LAB
***What is the ENTRYPOINT configured on the mysql image?***
- cat Dockerfile-mysql | grep ENTRYPOINT
***What is the final command run at startup when the wordpress image is run. Consider both ENTRYPOINT and CMD instructions***
- docker-entrypoint.sh apache2-foreground

```java
# Pull base image.
FROM ubuntu:14.04

# Install.
RUN \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y build-essential && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget && \
  rm -rf /var/lib/apt/lists/*

# Add files.
ADD root/.bashrc /root/.bashrc
ADD root/.gitconfig /root/.gitconfig
ADD root/.scripts /root/.scripts

# Set environment variables.
ENV HOME /root

# Define working directory.
WORKDIR /root

# Define default command.
CMD ["bash"]
```
***Run an instance of the ubuntu image to run the sleep 1000 command at startup.
Run it in detached mode.***
- docker run -d  ubuntu sleep 1000