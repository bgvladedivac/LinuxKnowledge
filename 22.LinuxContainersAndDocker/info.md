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
