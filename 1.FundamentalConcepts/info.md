## Kernel

Kernel is often used as a synonym for referring to the central software that manages and allocates computer resources(i.e, the CPU, RAM, and devices). The Linux kernel executable typically resides at the pathname **/boot/vmlinuz**.

**Tasks performed by the kernel**<br />

**Process scheduling**: pc has one or more CPUs, which execute the instructions of programs. Multiple processes(running programs) can simultaneously reside in memory and each may receive use of the CPUs(s). The rules  which processes receive use of the CPU and for how long are determined by the kernel process scheduler ( rather than the processes themselves ).<br />

**Memory management**: kernel employs virtual memory management. Processes are isolated from one another, so that one process can't read/edit the memory of another. Only part of a process needs to be kept in memory, lowering the memory requirements of each process and allowing more processes to be held in RAM simultaneously.<br />

**Provisiong of a file system**<br />
**Creation termination of processes**<br />

**Access to devices**: the devices(monitor, mice, disk ...) attached to a computer allow communication of information between the computer and the outside world, permitting I/O. The kernel provides programs with an interface that standadisez and simplifies access to devices.<br />

**Networking**

**Provisiong of a system call application programming inteface(API)**: processes can request the kernel to perform various tasks using kernel entry point known as **system call/s**.

## Shell
Program designed to read commands typed by a user and execute appropriate programs in response to those commands.

## Users and Groups
Every user of the system has a unique loing name and a corresponding numeric user ID(UID). For each user, there are defined by a line in the file **/etc/passwd**, also including groupId, home directory, login shell ...

Each group is identified by a single line in the file **/etc/group**, also including user list ...

**The superiuser** is known as **root**. He is able to bypasses all permission checks in the system.

