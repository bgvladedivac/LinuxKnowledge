## Process
A process is a running instance of a launched executable program. A processes consists of: <br />
* an address space of allocated memory. <br />
* security properties including ownership and priveleges. <br />
* state. <br />
* one or more execution threads. <br />

The **environment** of a process includes:
* local and global variables.<br />
* a current scheduling context.<br />
* allocated system resources, such as file descriptors and network ports.<br />

## How does a process is being created ?
An existing process(**parent**) duplicates its own address space(**fork**) to create a new **child**. Each process is assigned a unique process ID(**PID**). Through the **fork** route a child process inherits security identifies, previous and current file descriptors, port and resource privileges, environment variables and program code. Normally a parent process **sleeps** while the **child** process runs, setting a request(**wait**) to be signaled when the child completes.

## Process states
Each CPU(cpu core) can be working on one process at a single point. As a process runs, its requirements for CPU time and resource allocation change. Processes are assigned a state, which changes as circumstances require.

Name | Flag  |  Kernel-defined state |
--- | --- | --- |
Running | R | The process is either executing on a CPU or waiting to run. |
Sleeping | S | Waiting for some condition, hardware request, system resource access, signal ... |
Stopped | T | Has been stopped, usually by being signaled by a user or another process. |

## Listing processes
The **ps** is one of the ways used for listing processes. The command provides detailed information, including:<br />
* the user identification(**UID**) which determines process privileges. <br />
* the unique process identifier(**PID**). <br />
* the CPU and real time. <br />
* how much memory the process has allocated in various locations. <br />
* the location of process **STDOUT**, known as controlling terminal. <br />
* current process state. <br />
```{r, engine='bash', count_lines}
yum search KEYWORD 
```

