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

## Independence, think again
In a Linux system no process is independent of any other process. Every process in the system, except the initial process(**init**/**systemd**) has a parent process. New process are not created, they are copied or rather cloned from previous processes. Every **task_struct** representing a process keeps pointers to its parent process and to its siblings (those processes with the same parent process) as well as to its own child processes. You can see the family relationship between the running processes in a Linux system using the **pstree** command.


## Niceness/priority
In Linux we can set guidelines for the CPU to follow when it is looking at all tasks it has to do. These guidelines are called niceness or nice values. The Linux niceness scale goes from -20 to 19. The lower the number the more priority that task gets. You could use **renice** to change a niceness of a process.

## How does a process is being created ?
An existing process(**parent**) duplicates its own address space(**fork**) to create a new **child**. Each process is assigned a unique process ID(**PID**). Through the **fork** route a child process inherits security identifies, previous and current file descriptors, port and resource privileges, environment variables and program code. Normally a parent process **sleeps** while the **child** process runs, setting a request(**wait**) to be signaled when the child completes.

## Process states
Each CPU(cpu core) can be working on one process at a single point. As a process runs, its requirements for CPU time and resource allocation change. Processes are assigned a state, which changes as circumstances require.

Name | Flag  |  Kernel-defined state |
--- | --- | --- |
Running | R | The process is either executing on a CPU or waiting to run. |
Sleeping | S | Waiting for some condition, hardware request, system resource access, signal ... |
Stopped | T | Has been stopped, usually by being signaled by a user or another process. |
Terminated | Z | Process has just been terminated. |
Waiting for device | D | Uninterruptibly waiting for a device to respond |

Additional characters

Name | Flag  |   
--- | --- |  
< | high-priority |  
N | low-priority | 
s | session leader |


## Listing processes
The **ps** is one of the ways used for listing processes. The command provides detailed information, including:<br />
* the user identification(**UID**) which determines process privileges. <br />
* the unique process identifier(**PID**). <br />
* the CPU and real time. <br />
* how much memory the process has allocated in various locations. <br />
* the location of process **STDOUT**, known as controlling terminal. <br />
* current process state. <br />

Keep in mind that ff the arguments cannot be located (usually because it has not been set, as is the case of system processes and/or kernel threads) the command name is printed within square brackets.
```{r, engine='bash', count_lines}

[devil@client ~]$ ps -ef | more
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  1 15:26 ?        00:00:01 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2     0  0 15:26 ?        00:00:00 [kthreadd]
root         3     2  0 15:26 ?        00:00:00 [ksoftirqd/0]
root         4     2  0 15:26 ?        00:00:00 [kworker/0:0]
root         5     2  0 15:26 ?        00:00:00 [kworker/0:0H]


[devil@client ~]$ ps -aux | more
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  1.1  0.6 128164  6844 ?        Ss   15:26   0:01 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2  0.0  0.0      0     0 ?        S    15:26   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    15:26   0:00 [ksoftirqd/0]
root         4  0.0  0.0      0     0 ?        S    15:26   0:00 [kworker/0:0]
root         5  0.0  0.0      0     0 ?        S<   15:26   0:00 [kworker/0:0H

```

## Running jobs in the background
Any commmand can be started in the background by appending an ampersand(**&**) to the command line. The bash tracks jobs, per session in a table displayed with the **jobs** command. Background jobs can reconnect to the controlling terminal by being brought to the foreground using the **fg** command with the job ID(%job number).


## Process control using signals
A signal is a software interrupt delivered to a process. Signals report events to an executing program. 

Signal number | Definition  |  Purpose |
--- | --- | --- |
1 | Hangup | Report termination of the controlling process of a terminal. |
2 | Keyboard interrupt | Causes program termination. Can be blocked or handled. Sent by typing **Ctrl-c** |
9 | Kill, **unblockable** | Causes abrupt program termination. Cannot be blocked, ignored or handled. Always fatal. |
15 | Terminate | Causes program termination. Can be bocked, ignored or handled. The polite way to ask a program to terminate. |
20| Keyboard stop | Can be blocked, ignored or handled. Sent by typing **Ctrl-z** |

## Load average
The load average represents the perceived system load over a time period. Linux implements the load average calculation as a representation of expected service wait times, not only for CPU but also for disks and network I/O. Linux counts not only processes, but threads individually as separate tasks. The load number is a global counter calculation, which is sum-totaled for all CPUs. Linux counts each physical CPU core as a separate execution unit, logically represented and referred to as individual CPU. Check **/proc/cpuinfo** for the kernel representation of system CPUs.
