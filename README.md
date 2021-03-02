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



