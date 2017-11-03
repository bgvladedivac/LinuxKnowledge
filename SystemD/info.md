## Systemd
System startup and server processes are managed by the systemd System and Service Manager. Systemd provides a method for activating resources, server daemons and other processes, both at boot time and on a running system. 

Daemons are processes that wait or run in the background performing various tasks. Generally, daemons start automatically at boot time and continue to run until shutdown or until they are manually stopped. By convention, the names of many daemon programs end in the letter "d".

Systemd and its features:
* on-demand starting of daemons without requiring a separate service.
* a method of tracking related processes together by using Linux control groups.

## Units
The **units** are the main systemd object.
systemd records initialization instructions for each daemon in a configuration file that uses a declarative language. Unit file types
include **service**, **socket**, **device**, **mount**, **automount**, **swap**, **target**, **path**, **timer**, **snapshot**, **slice** and **scope**.

Unit files are loaded from a set of paths determined during compilation. 


Path | Description | 
--- | --- |
/etc/systemd/system | Local configuration | 
/run/systemd/system | Runtime units |
/usr/lib/systemd/system | Units of installed packages |

## Systemctl
**systemctl** is used to manage different types of units. A list of available units can be displayed with:
```{r, engine='bash', count_lines}
systectl -t help
```
Service status
```{r, engine='bash', count_lines}
[root@serverX ~]#systemctl status sshd.service
  sshd . service - OpenSSH server daemon
  Loaded : loaded (/usr/lib/systemd/system/sshd . service; enabled)
  Active: active ( running) since Thu 2S14-S2-27 11: 51:39 EST; 7h ago
  Main PID: 1S73 (sshd)
  CGroup : /system . slice/sshd . service
           L1073 /usr/sbin/sshd -D
  Feb 27 11: 51:39 servers. example . com systemd[l] : Started OpenSSH server daemon .
  Feb 27 11: 51:39 servers. example. com sshd[1S73] : Could not load host key: /et ... y
  Feb 27 11: 51:39 servers. example. com sshd [1S73] : Server listening on s.s.s.s ... .
  Feb 27 11: 51:39 servers. example. com sshd[1S73] : Server listening on :: port 22 .
  Feb 27 11: 53:21 servers. example . com sshd[127S] : error : Could not load host k . . . y
  Feb 27 11: 53:22 servers. example. com sshd[127S] : Accepted password for root f ... 2
```

```{r, engine='bash', count_lines}
systemctl is-active sshd
systemctl is-enabled sshd
systemctl --failed --type=service
```
Keyword | Description | 
--- | --- |
loaded | Unit configuration file has been processed. | 
active(running) | Running with one or more continuing processes. |
active(waiting) | Running but waiting for an event. |
enabled | Will be started at boot time. |
disabled | Will not be started at boot time. |

## Unit dependencies
Services may be started as dependencie of other ones. List dependencies: 
```{r, engine='bash', count_lines}
systemctl list-dependencies sshd
```
## Masking services
A system may have conflicting services installed. Example, network/NetworkManager or firewalld/iptables.
To prevent confusion, a service may be masked. Masking creates a link in the configuration directories so that if
the service is started, nothing will happen.
```{r, engine='bash', count_lines}
[root@serverX -]# systemctl mask network
ln -s ' /dev/null' ' /etc/systemd/system/network . service ' 
```

## Targets
Targets are used for grouping and ordering units. Systemd provides the so called **default target**, which is a state in which the OS will boot, example **graphical target** or **multi-user.target**.
