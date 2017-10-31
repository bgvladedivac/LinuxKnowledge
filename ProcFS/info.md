## The need
In older UNIXs, there was no easy way to analyze/edit properties of the kernel, to answer questions such as:
* How many processes are running on a system and who owns them ? <br />
* What files does a process have open ? <br />
* What files are currently locked, and which processes hold the locks ? <br />

## Procfs
In order to provide easier access to kernel info, modern UNIX implementations provide a **/proc** virtual file system. This file system resides under the **/proc** directory and contains various files that expose kernel information, allowing
processes to conveniently read that information, and change it in some cases, using normal file I/O system calls. The /proc file system is said to be virtual because the files and subdirectories that it contains don’t reside on a disk. Instead, the kernel creates them “on the fly” as processes access them.


## Obratining Information About a Process
For each process on the system, the kernel provides a corresponding dir named **/proc/PID**.

```{r, engine='bash', count_lines}
[root@client ~]# cat /proc/1/status | more
Name:	systemd
Umask:	0000
State:	S (sleeping)
Tgid:	1
Ngid:	0
Pid:	1
PPid:	0
TracerPid:	0
Uid:	0	0	0	0
Gid:	0	0	0	0
FDSize:	64
Groups:	
VmPeak:	  193700 kB
VmSize:	  128164 kB
VmLck:	       0 kB
```

File | Description | 
--- | --- |
cmdline | command line args | 
environ | Environment list NAME=value pairs |
exe | Symbolic link to file being executed. |
fd | Directory containing symbolic links to files opened by this process. |
mounts | Mount points for this process. |
task   | Contains one subdirectory for each thread in process. |

## More information
Various files and subdirectories under **/proc** provide access to system-wide information.

Directory | Information  | 
--- | --- |
/proc/net| Status information about networking and sockets. | 
/proc/sys/fs | Settings related to file systems. |
/proc/sys/kernel | Various general kernel settings. |

