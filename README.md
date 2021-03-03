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

5. To see what all docker containers are running, you can use the **ps** command.
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
- Take a debian image:                      ```docker run -it debian:stretch```
- Install git on it:                        ```apt-get update && apt-get install -y git```
- Get the Container ID:                     ```docker ps -a```
- Commit the docker container:              ```docker commit 7a72f6b4e077 pdittaka/debian:1.0.0```
- Spin a new container from this new image: ```docker run -it pdittaka/debian```

### Approach 2:
Dockerfile is a text document that contains all the instructions that are needed to assemble the image.
Each image will add a new layer.
Naming: ```Dockerfile```

Sample Dockerfile:
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
Image building: ```docker build -t dockerapp:v0.1 .```
Check available Images: ```docker images```
Run Docker in detached mode: ```docker run -d -p 5000:5000 IMAGE_ID```
Check running Docker containers: ```docker ps```
Running commands on the running containers: ```docker exec -it CONTAINER_ID bash```
Opening ```http:\\localhost:5000``` on development machine.


## Docker Container Links
Useful when one container needs to access or send information to the other container.
No ports need to be exposed externally to the container.
Useful for the world of Microservices.
TODO?.

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







