# What problem does Docker solve?.
In a new project or an existing project where a new person joins, it is difficult to set up development environments - particularly if there are different components like message queues, databases, servers involved. 
- There may be specific version of Tomcat to be installed with a specific version of JRE and a specific version of MySQL database.
- There could even be some environment variables that are set in the other developers machine which are forgotten to be communicated to the new developer.
- This problem happens all the while. This is what Docker is going to solve at its core.

## Advantages
- Removes compatibility problems: If Redis needs Java7 and RabbitMQ needs Java8, this is not a problem anymore.
- Standardizes the way components are made available for use: Using containers.
- Easier to set environments: Just pull relevant Docker containers.
- The same docker image can be containerized in different environments.
- Underlying OS can be upgraded/changed.
- If you want to scale up, all you have to do is to spawn a new docker image.

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

3. Docker container can also be run in an interactive manner using the **-i** and **-t** commands.
```
D:\Development\LearnDocker>docker run -i -t busybox:1.24
/ # echo Pavan
Pavan
/ # ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
/ #
```

4. It is interesting to note that any file created in one container are not persisted when that container is restarted. This is the foreground mode. Docker
can also be run in the background mode using the **-d** flag. This option stands for **detach mode**.
```
D:\Development\LearnDocker>docker run -d busybox:1.24 sleep 1000
c492ba8abe689e909e8b4261149f30cd9b0f8d39ae6ad04d0c0b1c9762f17b92
```

5. To see what all docker containers are running, you can use the **ps** command. Note that ```docker conainer ls``` is the latest command.
```
D:\Development\LearnDocker>docker ps
CONTAINER ID   IMAGE          COMMAND         CREATED          STATUS          PORTS     NAMES
2d2c47694f16   busybox:1.24   "sleep 10000"   8 seconds ago    Up 7 seconds              wonderful_spence
0cef827634f6   busybox:1.24   "sleep 10000"   14 seconds ago   Up 13 seconds             distracted_volhard
c492ba8abe68   busybox:1.24   "sleep 1000"    17 seconds ago   Up 16 seconds             lucid_elion
```

6. To see what all docker containers have run, use the **-a** flag.
```
D:\Development\LearnDocker>docker ps -a
CONTAINER ID   IMAGE          COMMAND                 CREATED          STATUS                      PORTS     NAMES
2d2c47694f16   busybox:1.24   "sleep 10000"           5 minutes ago    Up 5 minutes                          wonderful_spence
0cef827634f6   busybox:1.24   "sleep 10000"           5 minutes ago    Up 5 minutes                          distracted_volhard
c492ba8abe68   busybox:1.24   "sleep 1000"            5 minutes ago    Up 5 minutes                          lucid_elion
cb4e3eb1a0d1   busybox:1.24   "sh"                    8 minutes ago    Exited (0) 7 minutes ago              silly_blackwell
7859b458e276   busybox:1.24   "echo 'Hello World!'"   12 minutes ago   Exited (0) 12 minutes ago             condescending_meninsky
234610d116a5   busybox:1.24   "echo 'Hello World'"    14 minutes ago   Exited (0) 14 minutes ago             vibrant_euler
```

7. To remove a container after it is run, use the **--rm** command.
```
D:\Development\LearnDocker>docker run  --rm busybox:1.24 sleep 1

D:\Development\LearnDocker>docker ps -a
CONTAINER ID   IMAGE          COMMAND                 CREATED          STATUS                      PORTS     NAMES
bade6bb676b9   busybox:1.24   "sleep 10000"           46 seconds ago   Up 45 seconds                         sweet_meitner
2d2c47694f16   busybox:1.24   "sleep 10000"           8 minutes ago    Up 8 minutes                          wonderful_spence
0cef827634f6   busybox:1.24   "sleep 10000"           8 minutes ago    Up 8 minutes                          distracted_volhard
c492ba8abe68   busybox:1.24   "sleep 1000"            8 minutes ago    Up 8 minutes                          lucid_elion
cb4e3eb1a0d1   busybox:1.24   "sh"                    11 minutes ago   Exited (0) 10 minutes ago             silly_blackwell
7859b458e276   busybox:1.24   "echo 'Hello World!'"   15 minutes ago   Exited (0) 15 minutes ago             condescending_meninsky
234610d116a5   busybox:1.24   "echo 'Hello World'"    17 minutes ago   Exited (0) 17 minutes ago             vibrant_euler
```

8. We can name the docker container that is created using the **--name**.
```
D:\Development\LearnDocker>docker run --name sleep_1_box -d busybox:1.24 sleep 1
04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3
```

```
D:\Development\LearnDocker>docker ps -a
CONTAINER ID   IMAGE          COMMAND                 CREATED          STATUS                      PORTS     NAMES
04bcfdd6cbf7   busybox:1.24   "sleep 1"               5 minutes ago    Exited (0) 5 minutes ago              sleep_1_box
bade6bb676b9   busybox:1.24   "sleep 10000"           7 minutes ago    Up 7 minutes                          sweet_meitner
2d2c47694f16   busybox:1.24   "sleep 10000"           14 minutes ago   Up 14 minutes                         wonderful_spence
0cef827634f6   busybox:1.24   "sleep 10000"           14 minutes ago   Up 14 minutes                         distracted_volhard
c492ba8abe68   busybox:1.24   "sleep 1000"            14 minutes ago   Up 14 minutes                         lucid_elion
cb4e3eb1a0d1   busybox:1.24   "sh"                    17 minutes ago   Exited (0) 17 minutes ago             silly_blackwell
7859b458e276   busybox:1.24   "echo 'Hello World!'"   21 minutes ago   Exited (0) 21 minutes ago             condescending_meninsky
234610d116a5   busybox:1.24   "echo 'Hello World'"    23 minutes ago   Exited (0) 23 minutes ago             vibrant_euler
```

9. Inspecting docker container
```
D:\Development\LearnDocker>docker inspect 04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3
[
    {
        "Id": "04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3",
        "Created": "2021-03-02T16:30:58.0352782Z",
        "Path": "sleep",
        "Args": [
            "1"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-03-02T16:30:58.5402746Z",
            "FinishedAt": "2021-03-02T16:30:59.5445404Z"
        },
        "Image": "sha256:47bcc53f74dc94b1920f0b34f6036096526296767650f223433fe65c35f149eb",
        "ResolvConfPath": "/var/lib/docker/containers/04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3/hostname",
        "HostsPath": "/var/lib/docker/containers/04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3/hosts",
        "LogPath": "/var/lib/docker/containers/04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3/04bcfdd6cbf70b3cd550377cee8375867b3cf6aaa189606310b94619257ff7c3-json.log",
        "Name": "/sleep_1_box",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                30,
                120
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/65a755b36d91202eb803da71cac912adb659e53bdfd5dd8d00d78d4de3d8db95-init/diff:/var/lib/docker/overlay2/074be92fda90b1614e26ef93191c26632558cadf05d1391bcb66379b33b4a607/diff:/var/lib/docker/overlay2/2e408f08658bb375b0b815b9117570e257a9e02886774415fd594666f545418c/diff",
                "MergedDir": "/var/lib/docker/overlay2/65a755b36d91202eb803da71cac912adb659e53bdfd5dd8d00d78d4de3d8db95/merged",
                "UpperDir": "/var/lib/docker/overlay2/65a755b36d91202eb803da71cac912adb659e53bdfd5dd8d00d78d4de3d8db95/diff",
                "WorkDir": "/var/lib/docker/overlay2/65a755b36d91202eb803da71cac912adb659e53bdfd5dd8d00d78d4de3d8db95/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "04bcfdd6cbf7",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": [
                "sleep",
                "1"
            ],
            "Image": "busybox:1.24",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "ec4d99bd7adc1f1b0cd5c91a1f26a2f7404c4723c307457eaa5d9c1885755b70",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/ec4d99bd7adc",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "33ac612d09c8a12a580cc15496adff6b6ad54319ec9bbe7764245ade329e0546",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

10. Docker port mapping - An application running on a certain port in a Docker image could be mapped to another port on the host using this command.
Runs a Tomcat server on port 7777 ```docker run -it --rm -p 7777:8080 tomcat:8.0```
Runs a Tomcat server on port 8888 ```docker run -it --rm -p 8888:8080 tomcat:8.0```

# Docker Image Layers
Each Image is made up of layers. And each of the layers in itself is an Image.
Each of the layers add a certain aspect to realize the end goal of the image itself.

To know what all layers make up an Image, you can use ```docker history tomcat:8.0```

```
D:\Development\LearnDocker>docker history tomcat:8.0
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
ef6a7c98d192   2 years ago   /bin/sh -c #(nop)  CMD ["catalina.sh" "run"]    0B
<missing>      2 years ago   /bin/sh -c #(nop)  EXPOSE 8080/tcp              0B
<missing>      2 years ago   /bin/sh -c set -e  && nativeLines="$(catalin…   0B
<missing>      2 years ago   /bin/sh -c set -eux;   savedAptMark="$(apt-m…   17.7MB
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_ASC_URLS=https…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_TGZ_URLS=https…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_SHA512=cd8a4e4…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_VERSION=8.0.53    0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_MAJOR=8           0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV GPG_KEYS=05AB33110949…   0B
<missing>      2 years ago   /bin/sh -c apt-get update && apt-get install…   1.84MB
<missing>      2 years ago   /bin/sh -c set -ex;  currentVersion="$(dpkg-…   7.34MB
<missing>      2 years ago   /bin/sh -c #(nop)  ENV OPENSSL_VERSION=1.1.0…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV LD_LIBRARY_PATH=/usr/…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV TOMCAT_NATIVE_LIBDIR=…   0B
<missing>      2 years ago   /bin/sh -c #(nop) WORKDIR /usr/local/tomcat     0B
<missing>      2 years ago   /bin/sh -c mkdir -p "$CATALINA_HOME"            0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV PATH=/usr/local/tomca…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV CATALINA_HOME=/usr/lo…   0B
<missing>      2 years ago   /bin/sh -c set -ex;   if [ ! -d /usr/share/m…   159MB
<missing>      2 years ago   /bin/sh -c #(nop)  ENV JAVA_DEBIAN_VERSION=7…   0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV JAVA_VERSION=7u181       0B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV JAVA_HOME=/docker-jav…   0B
<missing>      2 years ago   /bin/sh -c ln -svT "/usr/lib/jvm/java-7-open…   33B
<missing>      2 years ago   /bin/sh -c {   echo '#!/bin/sh';   echo 'set…   87B
<missing>      2 years ago   /bin/sh -c #(nop)  ENV LANG=C.UTF-8             0B
<missing>      2 years ago   /bin/sh -c apt-get update && apt-get install…   1.95MB
<missing>      2 years ago   /bin/sh -c set -ex;  if ! command -v gpg > /…   0B
<missing>      2 years ago   /bin/sh -c apt-get update && apt-get install…   41.1MB
<missing>      2 years ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      2 years ago   /bin/sh -c #(nop) ADD file:8d73a09e59fe50289…   127MB
```

Each Image is read-only. When a container is created, it creates a new layer on top of the Image. This layer is a WRITABLE Layer.
It is to this layer, the container specific data is written to. This means that 
1. the data in this layer is removed as soon as the container is terminated.
2. multiple containers of the same image will share the layers that make up the image ( read-only layers ).

## Building a new Docker Image
You can build a new Docker Image using one of the two approaches:
1. Commit our change made in a container to create a new docker image
2. Use a Docker file.

### Approach 1:
Using docker commit statement:

Ex: Create a Debian image with Git installed on it.
- Take a debian image:                      ```docker run -it debian:stretch```
- Install git on it:                        ```apt-get update && apt-get install -y git```
- Get the Container ID:                     ```docker ps -a```
- Commit the docker container:              ```docker commit 7a72f6b4e077 pdittaka/debian:1.0.0```
- Spin a new container from this new image: ```docker run -it pdittaka/debian```

Ex: Create an Ubuntu image with Java installed on it.
- docker pull ubuntu
- docker container run -it ubuntu
  - apt-get update
  - apt-cache search jdk
  - apt-get install -y openjdk-8-jdk
  - docker commit -a "Pavan Dittakavi pawan.it17@gmail.com" 53e86a19fa3c
  - docker container run -it ff7297152274

**The downside of this approach to create new images is that the user of the image does not know what is inside that image. In the above example, how do I know what is in side that ubuntu java image apart from information in its name?.**

### Approach 2:
Dockerfile is a text document that contains all the instructions that are needed to assemble the image.
Each image will add a new layer.

Ex 1: Dockerfile
```
FROM ubuntu:latest
MAINTAINER Pavan Dittakavi "pawan.it17@gmail.com"
RUN apt-get update && apt-get install -y openjdk-8-jdk
CMD ["/bin/bash"]
```
Command: docker image build -t pdittaka/ubuntu_java .

Ex 2: This example also covers how to deploy a jar file to the container and run it.
- ENTRYPOINT and CMD are similar, except that CMD lets you override it via command line.
- MAINTAINER is deprecated.
- COPY and ADD are similar, although COPY is preferred.
```
FROM ubuntu:latest
MAINTAINER Pavan Dittakavi "pawan.it17@gmail.com"
RUN apt-get update && apt-get install -y openjdk-8-jdk
WORKDIR /usr/local/bin
COPY test-program.jar .
CMD ["java", "-jar", "test-program.jar"]
```

Ex 3:
```
FROM debian:stretch
RUN apt-get update
RUN apt-get install -y git
RUN apt-get install -y vim
```

1. Build context is sent to the Docker Daemon.
2. Get the debian image of that specific tag.
3. Docker starts new container and runs the apt-get command.
4. After commit is done, it removes the container created in step 3.
5. For the next step, a new container is created based on the new image in step 4 etc.
6. Basically containers are used as intermediaries in the build of the Image.
```
D:\Development\LearnDocker>docker build -t pdittaka/debian .
[+] Building 42.8s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                  0.2s
 => => transferring dockerfile: 134B                                                  0.0s
 => [internal] load .dockerignore                                                     0.1s
 => => transferring context: 2B                                                       0.0s
 => [internal] load metadata for docker.io/library/debian:stretch                     0.0s
 => [1/4] FROM docker.io/library/debian:stretch                                       0.1s
 => => resolve docker.io/library/debian:stretch                                       0.0s
 => [2/4] RUN apt-get update                                                          7.2s
 => [3/4] RUN apt-get install -y git                                                 28.9s
 => [4/4] RUN apt-get install -y vim                                                  5.4s
 => exporting to image                                                                0.9s
 => => exporting layers                                                               0.9s
 => => writing image sha256:9cb49594e31a99842c1ed898eff5dce1f24345b3c57af252bd9025a3  0.0s
 => => naming to docker.io/pdittaka/debian                                            0.0s
```

Docker File:
1. Each RUN instruction will create a new Image layer.
2. So chain RUN instructions in a Docker Image file to reduce the number of layers it creates.
Ex: Instead of this,
```
FROM debian:stretch
RUN apt-get update
RUN apt-get install -y git
RUN apt-get install -y vim
```
do this:
```
FROM debian:stretch
RUN apt-get update && apt-get install -y git && apt-get install -y vim
```
This prevents the number of layers that are created for the final Image.
3. Sort the Dockerfile instructions appropriately so prevent unintentional duplication.
4. CMD instructions specify what to run when the container starts up.
5. Docker caches Image layers built using the same instructions.
Ex:
```
D:\Development\LearnDocker>docker build -t pdittaka/debian .
[+] Building 0.1s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                  0.0s
 => => transferring dockerfile: 31B                                                   0.0s
 => [internal] load .dockerignore                                                     0.0s
 => => transferring context: 2B                                                       0.0s
 => [internal] load metadata for docker.io/library/debian:stretch                     0.0s
 => [1/4] FROM docker.io/library/debian:stretch                                       0.0s
 => CACHED [2/4] RUN apt-get update                                                   0.0s
 => CACHED [3/4] RUN apt-get install -y git                                           0.0s
 => CACHED [4/4] RUN apt-get install -y vim                                           0.0s
 => exporting to image                                                                0.0s
 => => exporting layers                                                               0.0s
 => => writing image sha256:9cb49594e31a99842c1ed898eff5dce1f24345b3c57af252bd9025a3  0.0s
 => => naming to docker.io/pdittaka/debian                                            0.0s
```
6. Cache can be disabled by using the ```--no-cache=true```
```
D:\Development\LearnDocker>docker build --no-cache=true -t pdittaka/debian .
[+] Building 45.4s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                                          0.0s
 => => transferring dockerfile: 31B                                                                           0.0s
 => [internal] load .dockerignore                                                                             0.0s
 => => transferring context: 2B                                                                               0.0s
 => [internal] load metadata for docker.io/library/debian:stretch                                             0.0s
 => CACHED [1/4] FROM docker.io/library/debian:stretch                                                        0.0s
 => [2/4] RUN apt-get update                                                                                  5.7s
 => [3/4] RUN apt-get install -y git                                                                         30.2s
 => [4/4] RUN apt-get install -y vim                                                                          8.5s
 => exporting to image                                                                                        0.9s
 => => exporting layers                                                                                       0.9s
 => => writing image sha256:b43b59894c6beb5c046e0cf9d5c71014382f10ec2f3d548ca438edc0111b915b                  0.0s
 => => naming to docker.io/pdittaka/debian                                                                    0.0s
```

7. COPY and ADD copies files to the container at a specific directory in the container.
8. Docker images can be pushed onto the repositories on DockerHub for use by other developers as well.
   - docker login
   - docker image push fleetman-webapp will fail because for all custom docker images, the user id is mandatory.
   - docker image tag 05c dockerloginid/appname.


## Dockerizing a web application

As an application, a Python web application can be dockerized easily. You just need to get a standard python installed docker image, install flask via pip, mark the working directory and run the .py file. The corresponding Dockerbuild file content is shown below:

```
FROM python:3.5
RUN pip install Flask==0.11.1
RUN useradd -ms /bin/bash admin
USER admin
WORKDIR /app
COPY app /app
CMD ["python", "app.py"] 
```
- Image building: ```docker build -t dockerapp:v0.1 .```
- Check available Images: ```docker images```
- Run Docker in detached mode: ```docker run -d -p 5000:5000 IMAGE_ID```
- Check running Docker containers: ```docker ps```
- Running commands on the running containers: ```docker exec -it CONTAINER_ID bash```
- Opening ```http:\\localhost:5000``` on development machine.

# Docker Container Links
Useful when one container needs to access or send information to the other container.
No ports need to be exposed externally to the container.
Useful for the world of Microservices.

Ex:
```docker run -d --name redis redis:3.2.0```
```docker run -d -p 5000:5000 --link redis dockerapp:v0.3```

## How linking works
The etc/hosts file shall be updated with container name along with its ip address.
That is why dockerapp is able to reach redis.

## Docker Compose
Manual linking of containers is impractical as our applications / number of containers increase.
Docker Compose is a tool for defining and running multi-container Docker applications.
A single configuration file docker-compose.yml is central.
```
D:\Development\LearnDocker\dockerapp>docker-compose version
docker-compose version 1.27.4, build 40524192
docker-py version: 4.3.1
CPython version: 3.7.4
OpenSSL version: OpenSSL 1.1.1c  28 May 2019
```
## docker-compose.yml
version: '3' // Version 3 format
services:  // Services that make up our application
  dockerapp:
    build: . // path to the Dockerfile used to buid the image
    ports:
      - "5000":"5000" // host:container
    depends_on:  // dependencies, if any
      - redis
  redis:
    image: redis:3.2.0

```docker-compose up``` will start all the services that make up the yml file and start the containers.

## Docker compose workflow
```docker-compose up -d``` runs in detached mode all the containers that are mentioned in the yml file.
```docker-compose ps``` lists all the runnig containers.
```docker-compose logs``` to see the logs for the containers
```docker-compose logs -f``` to see the updated logs for the containers
```docker-compose logs``` dockerapp to see the logs for a certain container.
```docker-compose stop``` to stop the containers.
```docker-compose rm``` to delete the containers.
```docker-compose``` build to build a new image, especially when you want to update an existing image. 

# Continous Integration
The practice where code is built as soon as a commit is made is CI. This helps in ensuring that the bugs are identified before production/staging deployments.
With Docker way of working, typically as soon as the code is comitted, a new docker image is created with the application and pushed to docker registry.
From that docker registry, it would be pulled and a container is spawned and run on staging/production servers.

Ex: Github commit -> CircleCI -> DockerHub repository

# Dockers in Production
Most deployments use VMs and run containers on top of them.
You can do a command line deployment onto a cloud provider like AWS or Digital Ocean.
```docker-machine create --driver digitalocean --digitalocean-access-token <xxxxx> docker-app-machine```

## Docker Swarm
In real production scenario, there would be lots of containers that have to be run on lots of machines. Docker Swarm is used for this purpose.
With Docker Swarm, a Manager Node delegates tasks to the Worker Nodes based on its scheduling.

```docker swarm init``` is used to initialize a swarm. The docker engine targetted by this command becomes the manager in the newly created single-node swarm.
```docker swarm join``` is used to join a swarm as a Swarm node.
```docker swarm leave``` is used to leave a swarm joined earlier.

## Docker Stack
Group of interrelated services that share dependencies that can be scaled together.

# Docker Networking
Docker0 is the bridge network used by Docker for communications from the outside world, to talk to containers.
Each container running on the docker host would connect to this Bridge.
Using this Bridge network interface, containers can interact with each other or with outside world.

```
C:\Users\Pavan Dittakavi>docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
edbadc3e911b   bridge    bridge    local
44301d5918a0   host      host      local
dd1af6b326ba   none      null      local
```
## None Network
No access to outside world, totally isolated.
Useful for scenario where no network access is needed.

```
C:\Users\Pavan Dittakavi>docker run -d --net none busybox sleep 1000
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
5c4213be9af9: Pull complete
Digest: sha256:c6b45a95f932202dbb27c31333c4789f45184a744060f6e569cc9d2bf1b9ad6f
Status: Downloaded newer image for busybox:latest
66c60b26d4fe7bd3810ac101f6c26e5518591419c3ed939f852d38fd575bc98e

C:\Users\Pavan Dittakavi>docker exec -it 66c60b26d4fe7bd3810ac101f6c26e5518591419c3ed939f852d38fd575bc98e /bin/ash
/ # ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
ping: sendto: Network is unreachable
/ # ifconfig
lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```
## Bridge Network
Default type of network in Docker.
All the containers in the same Bridge network are connected to each other.
They can connect to the outside world via the Bride network interface.
By default, a docker container would have two network interfaces - bridge and none.
```
C:\Users\Pavan Dittakavi>docker exec -it container_1 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:11 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:906 (906.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
          
C:\Users\Pavan Dittakavi>docker run -d --name container_2 busybox sleep 1000
5c3a3a6320cea6897be9e1c8dbf59b849b7b3648cc69b513d36c3cd618288b32

C:\Users\Pavan Dittakavi>docker exec -it container_2 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:03
          inet addr:172.17.0.3  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:656 (656.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)


C:\Users\Pavan Dittakavi>docker exec -it container_1 ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=3.664 ms
```
As per need, a new bridge network can be created.
```docker network create --driver bridge my_bridge_network```
This creates a new bridge.
You can create a new docker container with this new bridge network as follows:
```docker run -d --name container_3 --net my_bridge_network busybox sleep 1000```
Each bridge network is isolated from one another and so a ping from container_3 to container_1 does not resolve.
To allow this:
```doker network connect bridge container_3```
This will add one more network interface to container_3 so that it can ping container_1 now.

To disconnect this network interface, we can use the below command:
```docker network disconnect bridge container_3```

## Host Network
In this configuration, all the network interfaces defined on the host machine are accessible to the container.
These are called as open containers.

```docker run -d --name container_4 --net host busybox sleep 1000```

## Overlay Network
In cases where a network has to be formed across hosts.

Docker compose file is typically updated to point to different networks for different containers involved in a stack. This helps in network segregation.
Ex:
```
version: '2'
services:
  proxy:
    build: ./proxy
    networks:
      - front
  app:
    build: ./app
    networks:
      - front
      - back
   db:
     image: postgres
     networks:
      - back

networks:
  front:
    driver: custom-driver-1
  back:
    driver: custom-driver-2
    driver_opts:
      foo: "1"
      bar: "2"
```

# Volumes
- Volumes are mounted locations on Containers to store persistent information.
- They are needed as once the docker container terminates, it takes away all data that it generated.
- So to view logs etc, Volumes are created for each container so that the data of interest that is stored in these Volumes persist even after the container is killed. 

# Next steps
- Read more on Docker swarm and Docker stack.
- How is scaling done in a real world scenario?.
- Where is kubernetes coming into play?.
- Docker client, Docker daemon, Docker server, Docker host
- Docker MySQL Image and connect with an editor.
- Docker NodeJS and build a REST API.
- Custom Dockerfiles
- Custom Docker-compose
- Spawning an additional Docker container.

# Running Tomact on Docker container and expose on port 80

- docker pull tomcat
- docker run -it -p 80:8080 tomcat


# Hands-on

## Running a basic SpringBoot application inside Docker
- Build a jar for the SpringBoot project that hosts REST API.
- 1. Create a docker image file
```
FROM openjdk:11
MAINTAINER baeldung.com
COPY target/docker-message-server-1.0.0.jar message-server-1.0.0.jar
ENTRYPOINT ["java","-jar","/message-server-1.0.0.jar"]
```
- 2. Build a docker image from the dockerfile ```docker build --tag=restapi:latest .```
- 3. Run the docker container from the image ```docker run -p 8888:5000 restapi:latest```
- https://www.baeldung.com/dockerizing-spring-boot-application

## How to set environment variables in docker

## COPY vs ADD in Dockerfile
- ADD does a little more than COPY. For instance, when a tar/zip file is copied using ADD, it extracts automatically the zipped contents to the destination.
- So use COPY where ever possible and use ADD when explicitly needed.
- https://stackoverflow.com/a/24958548/3210526

## CMD vs ENTRYPOINT in Dockerfile
- One of ENTRYPOINT or CMD should be present in a Dockerfile.
- https://stackoverflow.com/a/39408777/3210526

## EXPOSE vs PUBLISH in Dockerfile
- EXPOSE is to let a port accessed by other containers - Inter Container Communication.
- PUBLISH is to let the port be accessed outside the container, say from host machine.
- If it is not EXPOSEd and PUBLISHed, then only that service is not accessible outside the container.
- ![image](https://user-images.githubusercontent.com/42272776/167265398-6e891bcc-3393-4a19-9c93-9d80d6d6a7c4.png)
- https://stackoverflow.com/a/22150099/3210526


## How to add more containers and put a load balancer ahead of them / application scaling

## Docker Network
- All containers that start on the same docker host would get added to the default bridge network.
```
"Containers": {
            "2f10b3507a772d6d61a3863cd604cda1a2544e555ec9fca1e2c69d975bad63e6": {
                "Name": "crazy_almeida",
                "EndpointID": "3a8e084a5187608a722be2210ef65b9a4c27af3c9ff16b97d26841a5c86dc1ec",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        }
```
- This scheme however lets containers access each other using their IP Address only. And since IP Addresses can change, this scheme is not robust.
- A better way is to create a network and join those containers that need access to this network & lets the containers access each other via their names.
- This is user-defined-network.
- Content is very well covered here: https://www.tutorialworks.com/container-networking/
- User Defined Bridge Network
  -  

## How do two containers connect with each other?
- https://www.tutorialspoint.com/docker/docker_networking.htm
- https://www.tutorialspoint.com/docker/docker_container_linking.htm


## Docker Volumes
- Used for persisting data.
- Mapping of host folders to container folders.
- ![image](https://user-images.githubusercontent.com/42272776/167266578-6c54617f-2c56-40f7-ba4c-40afe5fa480d.png)
- docker run -v command is used to create volumes.
- Docker compose can also be used to create volumes.
- TODO - what is the example

## What is Docker Compose?
- If there are multiple applications involved, like NGINX, MYSQL, starting them via individual dockerfiles may not be easy. It becomes more difficult as the number of components increase.
- So a better way to manage this is via Docker Compose.
- The intent is captured in an YML file.
- TODO https://www.tutorialspoint.com/docker/docker_compose.htm


## What is Docker Swarm?




## Commands

| Command      | Description |
| ----------- | ----------- |
| docker build --tag=restapi:latest . | Builds a Docker image based off a Dockerfile |
| docker run -p 8888:5000 restapi:latest | Runs a container for the Docker image | 
| docker run -d -p 8888:5000 restapi:latest | Runs a container for the Docker image in detached mode | 
| docker images     | Lists the images in local machine |
| docker ps   | Lists the containers that are running |
| docker stop 789fb42060e5 | Stops the container that is running, but does not delete it |
| docker rm 789fb42060e5 | Removed the container specified |
| docker rm -f 96f8fae6b216 | Stops and removes the specified container |
| docker inspect 7568ed0eb3b5 | Inspects the running container. Content/Settings of the container come up as JSON text |
| docker logs -f 7568ed0eb3b5 ![image](https://user-images.githubusercontent.com/42272776/167261055-4235655b-8f3a-4d4d-94b5-b71a7d5d6403.png) | The application logs as shown in terminal |
| docker stats 617bf86d4f32 | CPU/Memory usage for a specific container |
| docker cp config.xml 617bf86d4f32:/tmp ![image](https://user-images.githubusercontent.com/42272776/167263395-3517f668-c9ed-4ab2-8b23-5d94fa3ef5fe.png) | Copies config.xml from the host to /tmp directory on the container specified |
| docker cp 617bf86d4f32:/tmp/config.xml config_from_container.xml | Copies config.xml from the container /tmp directory to current host |
| docker exec -it 617bf86d4f32 sh ![image](https://user-images.githubusercontent.com/42272776/167263596-0fdaaefe-300c-4da1-abfe-5655780722f3.png) | Launches a shell onto the container |


More: https://www.tutorialspoint.com/docker/docker_quick_guide.htm



## Docker Best Practices
- https://www.youtube.com/watch?v=8vXoMqWgbQQ


