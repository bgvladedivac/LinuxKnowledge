## Linux containers
A container is a lightweight application isolation mechanism that allows the kernel to run groups of processes in their own isolated user spaces, seperate from host system. The container has its own process list, network stack, file systems ... but shares the kernel with the host and the other containers running on the system. Linux containers are implemented through a set of 3 kernel features: <br />

1. **Namespace** for isolation.<br />
2. **Control groups** for resource control.<br />
3. **SELinux** for security.

**Docker** is just a tool used to manage containers. Docker images are portable and can be saved and exported to other systems and users.
