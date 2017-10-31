## The need
In older UNIXs, there was no easy way to analyze/edit properties of the kernel, to answer questions such as:
* How many processes are running on a system and who owns them ? <br />
* What files does a process have open ? <br />
* What files are currently locked, and which processes hold the locks ? <br />

## Procfs
In order to provide easier access to kernel info, modern UNIX implementations provide a **/proc** virtual file system. This file system resides under the **/proc** directory and contains various files that expose kernel information, allowing
processes to conveniently read that information, and change it in some cases, using normal file I/O system calls. The /proc file system is said to be virtual because the files and subdirectories that it contains don’t reside on a disk. Instead, the kernel creates them “on the fly” as processes access them.


