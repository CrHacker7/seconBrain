- It will run an instance of the nginx app on the docker host if it already exists.

> [!tip] docker run
> - Start a container from an image.

> [!tip] docker ps
> Lists all running containers and some basic info 'bout 'em such as:
> - container ID
> - Name of the image
> - the current status and the name of the container
> > [!tip] docker ps -a
> > - This outputs all running, stopped or exited containers.

> [!tip] docker stop <...>
> - We must provide the container ID or the container name

> [!tip] docker rm silly_sammet
> - To remove a stopped or exited container permanently.
> > [!tip] docker rm 342 e0a 42d
> > It will remove all of them at once. We can provide the first few characters alone.


> [!tip] docker images
> - To see a list of available images and their sizes.

> [!tip] docker rmi nginx
>  - Delete all dependet containers to remove image.
>  - We must stop and delete all dependent containers to be able to delete

> [!tip] docker pull nginx
> - Only if we want to download and NOT running the container.

> [!warning] docker run ubuntu
> The container exits immediately because ubuntu is an image of an operating system that is used as the base image for other application. There is no process or app running in it by default.
> - Containers are not meant to host an operating system.
> - They are meant to run a specific task or process, such as to host an instance of a web server, or application server or a database, or simply to carry some kind of computation or analysis task.
> - Once the task is completed, the container exits.
> - A container only lives as long as the process inside it is alive.
> - IF THE WEB SERVICE INSIDE THE CONTAINER IS STOPPED OR CRASHED, THEN THE CONTAINER EXITS.
> >[!warning] docker run ubuntu sleep 5 (seconds)
> >- If the image isn't running any service, as ubuntu, we could instruct docker to run a process
> >- When the container starts, it runs the sleep cmd and goes into sleep for 5 seconds posts, which the sleep cmd exits and the container stops.

> [!warning] docker exec distracted_sammy cat /etc/hosts
> To see inside a running container, to execute a command on my docker container. In this case, to print the etc/hosts file.


> [!warning] docker run myfile/simple-webapp
> - It runs in the foreground or in an attached mode. We will be attached to the console, and we will see the output of the web service on the screen.
> - CTRL + C  exits and gets back to our prompt

> [!warning] docker run -d myfile/simple-webapp
> - It runs in the background or in an dettached mode. We will be back to our prompt immediatly

> [!warning] docker attach a043d
> - It runs in the background or in an dettached mode. We will be back to our prompt immediatly
> - If we are specifying the ID of a container in any docker command, we can provide the first few characters alone.
 
 hibernate persistencia, patrones de diseño
LAB
***Delete all containers from the Docker Host.
Both Running and Not Running ones. Remember you may have to stop containers before deleting them.***
- To stop containers run the command docker stop <container id | container name>
- And then to delete them run docker rm <container id | container name>
- To stop all the containers at once, run the command: docker stop $(docker ps -aq)
To remove all the stopped containers at once, run the command: docker rm $(docker ps -aq)
***delete the ubuntu image***
- docker rmi ubuntu
***Run a container with the nginx:1.14-alpine image and name it webapp***
- Run the command ***docker run -d --name webapp nginx:1.14-alpine*** and check the status of created container by docker ps command.
***Cleanup: Delete all images on the host***
- Run the command docker rmi <IMAGE:TAG>
- ***Stop and delete*** all the containers being used by images.
Then run the command to delete all the available images: ***docker rmi $(docker images -aq)***
