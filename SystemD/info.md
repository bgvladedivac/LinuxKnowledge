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

