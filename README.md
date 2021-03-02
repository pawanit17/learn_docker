# History
Earlier, each machine only used to run a single application. 
1.This is costly and also the CPU utilization is not efficient. 
2.Configuring server and migrations is not straightforward.

One improvement is Hypervisors. On top of a the host operating system, the Hypervisor (VMware, VirtualBox) is installed. This will create virtual environments where multiple other OSes can run - Ubuntu, Debian etc, each of which will be running their own application. AWS too is a set of Virtual boxes.
1. This is very cost effective.
2. Easy to scale.
3. Problem is each VM will have its own OS.
4. VM portability issues.

Docker is an container based virtualization technique. 
1. A single container engine runs on an OS that is capable of hosting multiple docker containers.
2. Docker containers do not have their own OS and instead relies on the host operating system.
3. Host can be a physical machine or a virtual machine.
4. Light weighted as each container does not have its own OS again.

# Why containers?.
We can technically run all applications on a single machine. But this does not provide **runtime isolation**.
Ex: If you have few components of an application ( say, REDIS ) that depend on one JRE version and another application ( say, Webserver ) that depend on another JRE, then it will be difficult to manage the server. You can put the different JREs in the boot up batch files, but still managing is difficult.
With containers, we can install JRE7 on one container, install REDIS and in another container, we can install JRE8 and install Webserver.

Benefits over VMs:
1. Efficient - usage and cost
2. Faster deployment
3. Portability.

# Architecture
1.Docker clients are the interface to end users that lets users run commands to talk to Docker Daemon.
2.Docker Daemon does the building, running docker containers. This is also called as Docker Server or Docker Engine.

On Linux, the Docker client, Docker daemon run on the same host. Docker Daemon uses Linux specific kernel features. So, on Windows, Docker daemon runs inside a docker machine, which is a light weight Linux VM that runs on Windows.

# Terminology
1.**Images** are read-only templates used to create containers.
  - They are created using the **docker build** command either ourselves or by other users.
  - Images are stored in **Docker Registry**, like DockerHub.
  - Instances of Images are **Containers**.
2.Applications run on containers and all binaries needed for running an application are in Containers.
3.Images are stored in Registries. This can be private or in DockerHub, the public registry.
  - Inside a registry, Images are stored in repositories.
  - A Docker repository is a collection of different version of docker images identified with the help of tags.
4.DockerHub - set of public Docker Images that you can use. It is available at http://hub.docker.com/
  - Official Images are well maintained by Docker company.
  - There can be other images for the same application which are unofficial.

# Commands
Docker Images are downloaded to local box upon request. On subsequent requests, it is first searched in local box and then in remote docker registry.

1. To find the list of docker images on local box, use the below command

```
D:\Development\LearnDocker>docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

2. To spin up a Docker container, use the docker run command.

```
D:\Development\LearnDocker>docker run busybox:1.24 echo "Hello World"
Unable to find image 'busybox:1.24' locally
1.24: Pulling from library/busybox
Image docker.io/library/busybox:1.24 uses outdated schema1 manifest format. Please upgrade to a schema2 image for better future compatibility. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/
385e281300cc: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:8ea3273d79b47a8b6d018be398c17590a4b5ec604515f416c5b797db9dde3ad8
Status: Downloaded newer image for busybox:1.24
Hello World
```
The command says to create a container from the busybox docker image of version 1.24 and then run the echo command on top of it.
```
D:\Development\LearnDocker>docker run busybox:1.24 echo "Hello World!"
Hello World!
```
Subsequent runs do not download the image from the central repository again and will be very fast.
```
D:\Development\LearnDocker>docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
busybox      1.24      47bcc53f74dc   4 years ago   1.11MB
```
Docker run would list this downloaded image when queried.

Docker container can also be run in an interactive manner using the **-i** and **-t** commands.
```
D:\Development\LearnDocker>docker run -i -t busybox:1.24
/ # echo Pavan
Pavan
/ # ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
/ #
```

It is interesting to note that any file created in one container are not persisted when that container is restarted. This is the foreground mode. Docker
can also be run in the background mode using the **-d** flag.
```
D:\Development\LearnDocker>docker run -d busybox:1.24 sleep 1000
c492ba8abe689e909e8b4261149f30cd9b0f8d39ae6ad04d0c0b1c9762f17b92
```





