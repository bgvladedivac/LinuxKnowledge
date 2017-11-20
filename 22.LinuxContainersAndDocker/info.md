## Linux containers
A container is a lightweight application isolation mechanism that allows the kernel to run groups of processes in their own isolated user spaces, seperate from host system. The container has its own process list, network stack, file systems ... but shares the kernel with the host and the other containers running on the system. Linux containers are implemented through a set of 3 kernel features: <br />

1. **Namespace** for isolation.<br />
2. **Control groups** for resource control.<br />
3. **SELinux** for security.

**Docker** is just a tool used to manage containers. Docker images are portable and can be saved and exported to other systems and users.

## Namespaces
The kernel provides container isolation thorugh namespaces, which create a new environment with a uniquq subset of the resources on the system. There are mainly 5 namespaces currently in use by containers:<br />

1. **Mount** isolates the file systems seen by the container. Containers have a different */* than each other and also different than the host system.
2. **PID** helps to isolate the process ID table for each container. Processes inside a container cannot see outside processes. 
3. **Network** helps to isolate the network. Each container has its own network interfaces, routing table and firewall rules, which are not directly visible from the hosts's default network namespaces or each other. They can be connected to the hosts's network infrastructure and the outside world.
4. **IPC** isolates the interprocess communication channel(*IPS*) resources such as *System V shared memory*. Two containers cannot interact with each other's shared memory.
5. **UTS** targets the host names. A container can have a different host name and domain name than the containers and host system.

## Control groups
Control groups are used by the kernel to manage system resources. Cgroups allow adjustable allocation of CPU time, memory and I/O bandwidth among processes and groups of processes. Containers use *cgroups* to manage resource consumption, so that a container will get a certain share of system resources but not steal all system resources.

## SELinux
In order to protect the host and other containers from a compromised container, *SELinux* is used. With SELinux *enforcing*, container processes can only write to container files. Container processes run as the type *svirt_lxc_net_t*, and image files are labeled with the type *svirt sandbox_file t*. 

## Docker
Docker provides the user interface, API, image format and other tools used to manage Linux containers. Docker runs a daemon started by the systemd unit *docker.service* to manage containers. The user interacts with this daemon through the docker command. Images are stored in a local index kept in the */var/lib/docker* directory, but are loaded and exported with the docker command. 

**A Docker image** is a static snapshot of a container's configuration, which is used to launch a container. The image is a read-only layer that is never modified. Instead, Docker adds a readwrite overlay to which all changes are made. Changes are saved by creating a new image. A single image can be used to generate many containers that are very slightly different, but only need enough disk space to store a very small amount of differences.

## Containers and virtualization
Containers and virtualization are two technologies that provide different ways of dividing up system resources. Containers can be run on virtual machines and in cloud computing environments, combining both technologies. There are use cases more suited for one technology or the other. 

Some advantages of **Docker** containers: 

1. Containers are *lightweight* in resource use, so more containers can be run on the same hardware. 

2. Containers can be *created and destroyed more quickly* than virtual machines.

3. Unlike virtual machines, containers do not need to support an entire operating system only a core runtime is needed for the application. This allows rapid application deployment.

4. Docker images have a version control stream, so successive versions of an image can be tracked and even reverted. Components reuse components from other layers, making container images very lightweight. 

5. Docker images are easily transferable to other machines which have Docker available.

Some advantages of **virtualization**: 

1. Virtual machines run their own kernel and full operating system, which allows stronger isolation between the host hypervisor and the virtual machine. 

2. Virtual machines can easily run operating systems and kernels that are completely different than the hypervisor host's operating system. 

3. Virtual machines can be live-migrated from one hypervisor node to another while running, containers must be stopped before being moved from one machine to another.
