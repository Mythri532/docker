# Overview of Docker

**Docker**<br>
Docker is a containerization platform which packages your application and all its dependencies together in the form of containers so as to ensure that your application works seamlessly in any environment, be it development, test or production

**Docker container**<br>
Docker containers include the application and all of its dependencies. It shares the kernel with other containers, running as isolated processes in user space on the host operating system. Docker containers are not tied to any specific infrastructure: they run on any computer, on any infrastructure, and in any cloud.

# Docker Architecture components

**a) Docker Daemon** - listens to Docker API requests and manages Docker objects such as images, containers, networks  and volumes. 

**b) Docker Clients** - With the help of Docker Clients, users can interact with Docker. 

**c) Docker Host** - provides a complete environment to execute and run applications. It comprises of the Docker daemon, Images, Containers, Networks, and Storage. 

**d) Docker Registry** - Docker Registry manages and stores the Docker images. Public Registry is also called as Docker hub.Private Registry - It is used to share images within the enterprise. 

**e) Docker Images** - are read-only templates that you build from a set of instructions written in Dockerfile. 

Architecture is referred in 

**Workflow of Dock


# Installation of docker in centos machine. 

Set up the repository 

  1. sudo yum install -y yum-utils 

  2. sudo yum-config-manager \ 
    --add-repo \ 
    https://download.docker.com/linux/centos/docker-ce.repo 

  3. sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin 

  4. sudo systemctl start docker 

  5. sudo docker run hello-world 
   

# Docker commands:

1. To start docker<br> 
   systemctl start docker<br>  

2. To pull image from repository<br> 
   docker pull centos<br>
   docker pull ubuntu<br>

3. To show all images<br>
   docker images<br>

4. Creates the container named mycontainer from centos image<br>
   docker create --name mycontainer centos<br>
   creates the container named mycontainer from centos image<br> 

5. To delete docker container<br>
   docker rm <containername><br>

6. To remove image<br>
   docker rmi <imagename><br> 

7. To run a container<br>
   docker run [OPTIONS] IMAGE [COMMAND] [ARG...]<br> 
   docker run <image-name><br>
   note: if you use an image that is not in your local else it pull the image from  docker registry.<br> 
   docker create --name mycontainer <imagename><br>
   docker create --name mycontainer <imageid><br> 

8.To run a container in attached mode(foreground) and in detached mode (background)<br>

   By default, Docker runs the container in attached mode. Meaning it’s attached to the terminal session, where it displays output and messages.<br>

if you want to keep the container and current terminal session separate, you can run the container in the background using the -d attribute 

   docker container run -d [docker_image]<br>

 9.To run a container interactively 

This allows you to execute the commands inside the container 
   docker container run -it [docker_image] /bin/bash<br>  

10.Run a container and publisher container port 

When you run a container, the only way to access the process is from inside of it. To allow external connections to the container, you have to open (publish) specific ports. 
  -p [host_ip]:[host_port]:[container_port]<br>
   docker container run -p 8080:80 [docker_image]<br>

11.To start the container 
   docker start [OPTIONS] CONTAINER [CONTAINER...]<br> 
   docker start –a <container-name><br> 

12.To stop the container 
   docker stop <container-name><br>

13.To restart a container- stops and restart a container 
   docker stop <container-name><br>

14.Shows only the running container. 
   docker ps<br>  

15.docker ps –a ->shows all stopped and running containers.<br> 

16.container creation<br>
   docker container create –it –name <container-name> <image-name><br> 

17.The exec command runs a new command in a running container.<br>
   docker exec <container-name>  <command name> <br>
   docker exec <container-name>  ls <br>

18. Docker exec command which is used to run the command inside a container or to go inside a container.<br> 
   docker exec –it test /bin/bash <br>
   To effectively interact with the container this command is used.<br>

19. docker commit <br>
   Instead of launching a new container from zero, we can commit the old Docker container to create a new Docker image and use that to start a new container with the right ports open.<br>

   docker stop <container-name> <br>
   docker commit <container-name> <new-container-name><br> 
   docker rm <container-name> <br>
   docker container rm –force <containername><br>
   docker run -d --name httpd-container httpd <br>

20. Attach command connects our terminal to running container  

   docker container attach [options] container<br>

21.Default mode - The above command links the standard output (stdout), and the standard error (stderr) streams with our terminal. So, we can see the console output of the container in our terminal.<br> 

   docker run --name test_redis<br> 

22. Interactive mode – Let us to interact with the container from out terminal. -I option attaches a standard input of the bash shell to the container.<br> 

23.Detached mode - -d option which lets the container to run in background.<br> 

   So whenever required we can connect to container using its id and name.<br> 

24.Docker Build<br>

  This command which is used to build a docker image from a docker file.The build context is set of files specified in the path or url.<br>  

Url parameter – which can be git repositories,tar ball contexts,text files.<br> 

   docker build dockerfile<br>

25.docker container rm --force mycontainer1<br> 

   To delete the running container forcefully.<br> 

 26.docker container cp<br>

   This command copies file from container to host machine or from host machine to container.<br> 

  docker container cp   <containerid>:/src path   /destination path<br> 
  docker container cp /srcpath  containerid:/destination path.<br>

27.docker container create image-name.<br>
   This command which creates the container and but does not start it.<br> 
   docker containr create nginx<br>

28.docker logs <cont-d><br>
  To show the logs of the container.<br> 

29.docker inspect <cont-id><br> 
  To check the state of the container.<br> 

30.docker checkpoint<br> 
   checkpoint and Restore is an experimental feature that allows you to freeze a running container by checkpointing it<br>
   which turns its state into a collection of files on disk. Later, the  container can be restored from the point it was frozen.<br> 
   Usage:  docker checkpoint create [OPTIONS] CONTAINER CHECKPOINT<br>

  Create a checkpoint from a running container<br>
  --leave-running=false - Leave the container running after checkpoint<br> 
  --checkpoint-dir  - Use a custom checkpoint storage directory.<br>
  Inspect changes to files or directories on a container’s filesystem<br>

31.docker container export –o <gzfilename> <containername><br>
  This command is used to export the container in gz file.<br>

32.docker container inspect <container name> <br>
  docker inspect command which provides detailed info about container<br> 

33.docker container logs [options] container-name<br> 
   Fetches the container logs  
   docker container ls [options] <br>
   docker container ls -a <br> 
   Lists the running container by default and lists all container <br>  

34.docker container pause <container-name> <br>
   Pause all processes or container with one or more processes.<br> 

35.docker container port <container-name> private_port [/proto]<br>
   List port mappings for a container.<br>

36.docker container prune <br>
   Removes stopped container <br>

37.docker  container rename <container-name> <new-name>.<br> 
   Renames a container name <br>

38.docker container restart  <container –name> <br>
   Restart one or more container. <br>

39.docker container rm [options] container-name<br> 
   Removes one or more container. <br>

 40.docker container rm  --force <container–name><br>  
   To remove the running container forcefully.<br>

.41.docker diff<br>      
   Inspect changes to files or directories on a container’s fileeystem.<br> 
  
42.docker info<br> 
   This command displays system wide information regarding the Docker installation.<br>  

43.docker kill <container1> <con2> …<br> 
   Kills one or more container.<br> 

44.docker load --input fedora.tar<br> 
   Load an image from a tar archive or STDIN<br>    

45.docker login<br> 
   Login to a registry . Need to enter docker id and password in order to pull and push from the registry.<br>  

46.docker top <contid><br>  
   List the top processes in container<br>  

47.docker pause <contid><br>  
   To pause the container and unpause the container.<br>  

48.Systemctl docker stop<br>  
   To stop the docker<br>  

# DOCKERFILE 

Docker file is a file which consists set of instruction to create a docker image.<br>  

Docker file instruction command<br>  

**FROM** – It tells docker from which base image you want to base your image from.<br> 
**WORKDIR** – sets the current working directory inside the container equivalent to running cd.<br>  
**RUN** – executes the given command inside the container.<br>  
**COPY**- copies the file or directory from local host machine to container file system<br>  
**CMD**- specifies a command to execute when the image is run. CMD is the command the container executes by default when you launch the built image.<br> 
**MAINTAINER**- command is the person who is going to maintain this image. Here you specify the  keyword and just mention the email ID.<br> 
**ENTRYPOINT** - The ENTRYPOINT instruction works very similarly to CMD in that it is used to specify the command executed.<br> 
 when the container is started. However, where it differs is that ENTRYPOINT doesn't allow you to override the command.<br> 
**ADD** - The ADD instruction copies new files, directories or remote file URLs from source and adds them to the filesystem of the imag<br> 
 at the  path destination.<br> 
**User** - The USER instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.<br> 
**Onbuild** - The ONBUILD instruction adds to the image a trigger instruction to be executed at a later time,<br>  
when the image is used as the base for another build. The trigger will be executed in the context of the downstream build, <br> 
as if it had been inserted immediately after the FROM instruction in the downstream Dockerfile.<br> 
**Label** -The LABEL Dockerfile instruction adds metadata to an image. A LABEL is a key-value pair. To include spaces within a LABELvalue, use<br>  quotes and backslashes as you would in command-line parsing.<br> 
Example - LABEL "com.example.vendor"="ACME Incorporated"<br> 
**ARG** - The ARG instruction defines a variable that users can pass at build-time to the builder with the docker buildcommand using the <br> --build-arg varname=value flag. A Dockerfile may include one or more ARG instructions.<br> 
SHELL ["executable", "parameters"]<br> 
**Shell** - The SHELL instruction allows the default shell used for the shell form of commands to be overridden. The default shell on Linux is<br> ["/bin/sh", "-c"], and on Windows is ["cmd", "/S", "/C"]. The SHELL instruction must be written in JSON form in a Dockerfile.<br> 
**Healthcheck** - The HEALTHCHECK instruction tells Docker how to test a container to check that it is still working. This can detect cases<br>  such as a web server that is stuck in an infinite loop and unable to handle new connections, even though the server process is still running.<br> 
**Expose** - The EXPOSE instruction tells Docker that the container listens on the specified network ports at runtime. To actually publish the<br>  port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all<br>  exposed ports and map them to high-order ports.<br> 
**ENV** - The ENV instruction sets the environment variable key to the value value.<br> 
**Volume** - The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from <br> native host or other containers. The value can be a JSON array, VOLUME "/var/log/", or a plain string with multiple arguments,<br>  
such as VOLUME /var/log or VOLUME /var/log /var/db.<br> 

Once the dockerfile is ready we can execute docker build command to build an image from docker file.<br> 

**docker build –t image-name:tagname <dir> or docker build –t dockerfile.**<br> 

**-t** − is to mention a tag to the image<br> 
**ImageName** − This is the name you want to give to your image.<br>  
**TagName** − This is the tag you want to give to your image.<br>  
**Dir** − The directory where the Docker File is present.<br> 

**Examples**<br> 

FROM ubuntu <br> 
MAINTAINER demousr@gmail.com<br>  
RUN apt-get update <br> 
RUN apt-get install –y nginx<br>  
CMD [“echo”,”Image created”]<br> 

FROM alpine<br>  
RUN apk update <br> 
RUN apk add wget <br> 
RUN rm -rf /var/cache/apk/*<br>  
WORKDIR /root/ <br> 
ENTRYPOINT [ "wget"] 
CMD ["--help"]

FROM httpd:2.4
LABEL AUTHOR=user@example.com
LABEL VERSION=0.1
COPY mypage.html /usr/local/apache2/htdocs/mypage.html
WORKDIR /usr/local/apache2
COPY mypage.html htdocs/mypage.html

FROM ubuntu 
RUN apt-get update 
RUN apt-get install –y apache2 
RUN apt-get install –y apache2-utils 
RUN apt-get clean 
EXPOSE 80 CMD [“apache2ctl”, “-D”, “FOREGROUND”]

# Dockerize an java application
 
1.mkdir javaapp<br>

2.Add the below lines in vi javaapp<br>
public class Sample {<br>
        public static void main(String[] args) {<br>
        System.out.println("Hello, World!");<br>
    }<br>

}<br>

3.Add the below lines in dockerfile.<br>

FROM openjdk:11
COPY . /var/www/java
WORKDIR /var/www/java
RUN  javac Sample.java
RUN useradd nonroot
USER nonroot
CMD ["java","Sample"]

From the built image docker container can be created . 

# Docker volumes. 

Docker volumes which is used to persist data backup.To define Docker Volumes, they are file systems that can be mounted on Docker containers. 
They help in preserving the data and are independent of the container life cycle. One of the major advantages of Docker Volumes is that it allows the developers to backup their data and also allows easy sharing of file systems among Docker containers.  

It is also possible to mount the same volume to different containers and this allows easy sharing of data between them and this can be easily achieved with the use of simple commands and flags. 

# Creation of docker Volume 

sudo docker volume create <volume-name> 

Docker creates a particular directory for volume on the local machine 

List all the docker volumes. 
sudo docker volume list 

Inspecting a docker volume 

sudo docker volume inspect <volume-name> 

Mounting docker volumes. 

sudo docker run −−mount source=<name of volume>,destination=<path of a directory in container>/volumename <image_name> 

For example
sudo docker run −it −−mount source=myVolume,destination=/usr/src/app/myvolume ubuntu 

In order check the volume for stopped container. You need to start the container and need to execute docker exec –it <container-name> /bin/bash command. 
You can mount a docker volume to a docker container using the mount –flag. when you are running the Docker run command. You can also mount the same volume to multiple Docker containers and all the containers would have a shared access to the volume.   

**Deleting a docker volume** 
 
 In order to delete a docker volume, you need to ensure that the volume is not in use at that moment. If a container is running with the volume mounted in it, you would have to stop the container first before removing the mounted volume. After you have stopped the container, you can use the following command to remove the volume. 

sudo docker rm <name of volume> 

In order to delete all the volumes at once, make sure that none of the volumes is currently in use  

sudo docker volume prune. 

Sharing a Docker Volume with multiple Docker Containers 
If you want to share multiple files with multiple docker container ,you can put your files in a docker volume mount that volume with multiple containers and get shared access to that volume. 

sudo docker volume create  myvolume 
sudo docker run –it –name=container1 –mount source=myvolume,destination=/app ubuntu 
This will create a volume myvolume and mount this volume to a container called container1 of ubuntu image at a destination /app 

**How to mount a volume of host directory to docker container**

docker run -it  --name container1 –v /home/centos:/datavolume ubuntu 

docker run –it –name <container name> -v <path of the host directory>:/datavolume ubuntu 

**How to share the voume from one container to other container** 

docker run –it –volumes-from container1 –name container2 

**How to push the images to your account in docker hub.** 
docker login 
username and password should be provided.
Once login is succeeded proceed with other steps. 

docker tag <imageid> username/<imagename>:tagname 

docker tag ab736043b5ac 76625/dockerfile:firsttry 

docker push <dusername>/imagename 

# Docker Network 

Docker network which is used to connect two containers in a network. For Docker containers to communicate with each other and the outside world via the host machine, there has to be a layer of networking involved. Docker supports different types of networks, each fit for certain use cases. 

**The Bridge Driver**

This is the default. Whenever you start Docker, a bridge network gets created and all newly started containers will connect automatically to the default bridge network. 

**The Overlay Driver** 

The Overlay driver is for multi-host network communication, as with Docker Swarm or Kubernetes. It allows containers across the host to communicate with each other without worrying about the setup. Think of an overlay network as a distributed virtualized network that’s built on top of an existing computer network. 

**Host Networking**

If you use the host network driver for a container, that container’s network stack is not isolated from the Docker host. For instance, if you run a container which binds to port 80 and you use host networking, the container’s application will be available on port 80 on the host’s IP address. 

**Macvlan Network** 

Some applications, especially legacy applications or applications which monitor network traffic, expect to be directly connected to the physical network. In this type of situation, you can use the macvlan network driver to assign a MAC address to each container’s virtual network interface, making it appear to be a physical network interface directly connected to the physical network. In this case, you need to designate a physical interface on your Docker host to use for the Macvlan, as well as the subnet and gateway of the Macvlan 

# Network commands

1.docker network ls - This command can be used to list all the networks associated with Docker on the host. 

2.docker network inspect networkname - If you want to see more details on the network associated with Docker, you can use the Docker network inspect command. 
 sudo docker network inspect bridge 

3.Creating Your Own New Network- One can create a network in Docker before launching containers. 
 docker network create –-driver drivername name  

4.Attach the new network when launching the container. 
 sudo docker run –it –network=new_nw ubuntu:latest /bin/bash 

 docker network create network-name 
 docker run –net mynet –name server1 –dit ubuntu 
 docker run –net mynet –name server2 –dit ubuntu 
 docker inspect server1 | grep IPADRESS 
 docker inspect server1 | grep IPADRESS 
 Go inside server1 container docker exec –it server1 bash 
 Then ping ip address of another container 

# Container linking 

Container Linking allows multiple containers to link with each other. It is a better option than exposing ports. Let’s go step by step and learn how it works. 

sudo docker jenkins pull 

Sudo docker run  --name=jenkinsa –d jenkins 

docker run –name=reca –link=jenkinsa:alias-src –it ubuntu 

Now attach to the receiving container docker attach reca 



# Docker-compose 

Docker Compose is used to run multiple containers as a single service. 

For example, suppose you had an application which required NGNIX and MySQL,  

you could create one file which would start both the containers as a service without the need to start each one separately. 

Docker-compose Installation 

Step 1: curl -L "https://github.com/docker/compose/releases/download/1.10.0-rc2/dockercompose 
   -$(uname -s) -$(uname -m)" -o /home/demo/docker-compose 

Step 2: Next, we need to provide execute privileges to the downloaded Docker Compose file, using the following command   

chmod +x /home/demo/docker-compose 

Step 3: docker-compose version 

Creating the docker-compose file 

sudo vi docker-compose.yml 

version: 2
services:
  databases:
    image: mysql
    ports:
    - "3306: 3306"
    environment:
    - MYSQL_ROOT_PASSWORD=password
    - MYSQL_USER=user
    - MYSQL_PASSWORD=password
    - MYSQL_DATABASE=demodb
  web:
    image: nginx

The database and web keyword are used to define two separate services. One will be running our mysql database and the other will be our nginx web server.The image keyword is used to specify the image from dockerhub for our mysql and nginx containers.For the database, we are using the ports keyword to mention the ports that need to be exposed for mysql and then, we also specify the environment variables for mysql which are required to run mysql. 

services:
  webapp:
    image: examples/web
    ports:
      - "8000:8000"
    volumes:
      - "/data"






