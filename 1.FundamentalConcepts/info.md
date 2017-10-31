## Kernel

Kernel is often used as a synonym for referring to the central software that manages and allocates computer resources(i.e, the CPU, RAM, and devices). The Linux kernel executable typically resides at the pathname **/boot/vmlinuz**.

**Tasks performed by the kernel**
**process scheduling**: pc has one or more CPUs, which execute the instructions of programs. Multiple processes(running programs) can simultaneously reside in memory and each may receive use of the CPUs(s). The rules  which processes receive use of the CPU and for how long are determined by the kernel process scheduler ( rather than the processes themselves ).
**memory management**: kernel employs virtual memory management. Processes are isolated from one another, so that one process can't read/edit the memory of another. Only part of a process needs to be kept in memory, lowering the memory requirements of each process and allowing more processes to be held in RAM simultaneously. 
**provisiong of a file system**
**creation termination of processes**
**access to devices**: the devices(monitor, mice, disk ...) attached to a computer allow communication of information between the computer and the outside world, permitting I/O. The kernel provides programs with an interface that standadisez and simplifies access to devices.


not work after the update. 
```{r, engine='bash', count_lines}
yum update PKGNAME 
```
