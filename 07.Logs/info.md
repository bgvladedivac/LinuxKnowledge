## System logging
Processes and the kernel need to be able to record a log of events that occur. These logs can be useful for auditing the system and troubleshooting problems. By convention the **/var/log** directory is where these logs are persistently stored.
<br />

A standart logging system based on the Syslog protocol is built. Syslog messages are handled by two services, **systemd-journal** and **rsyslog**. 

## Systemd-journald
The systemd-journald daemon provides an improved log management service that collects messages from the kernel, the early stages of the boot process, standart output and error of daemons as they start up and run, and syslog. It writes these messages to a structured journal of events that by default does not persist between reboots. This allows syslog messages and events which are missed by syslog to be colleced in one central database. The syslog messages are also forwarded by systemd-journald to rsyslog for further procesing. <br />

The **rsyslog** service then sorts the syslog messages by type(facility) and priority(severity) and writes them to persistent files in the /var/log. <br />

The */var/log* directory holds various system and service specific  log files maintained by *rsyslog*.

Log file | Description | 
--- | --- |
/var/log/messages | Generic log file | 
/var/log/secure | The log file for security and authentication-related messages/errors. |
/var/log/cron | The log file related to periodically executed tasks. |
/var/log/boot.log | Messages related to system startup. |
 
## Syslog files
Programs use the syslog protocol to log events to a system. Each log message is categorized by a facility and priority.
Priority | Severity | 
--- | --- |
emerg | System is unsusable | 
alert | Action must be taken immediately. |
crit | Critical condition. |
For more information, check the man pafe of **rsyslog.conf**.



```{r, engine='bash', count_lines}
echo N > /sys/class/backlight/acpi_video0/brightness

```
