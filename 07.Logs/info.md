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
Programs use the syslog protocol to log events to a system. Each log message is categorized by a facility and priority. For example, the code 4 stands for priority *warning* and facility *warning condition*. For more information, check the man pafe of **rsyslog.conf**. The rsyslogd service uses the facility and priority of log messages to determine how to handle them. This is configured by the file **/etc/rsyslog.conf** and by ***. conf** files in **/etc/rsyslog.d**. 
<br />

The **#### RULES ####** section of */etc/rsyslog.conf* contains directives that define where log messages are saved. The left side of each line indicates the facility and severity of the log message the directive matches. The rsyslog.conf file can contain the character * as a wild ca rd in the facility and severity field, where it either stands for all facilities or all severities. The right
side of each line indicates what file to save the log message in. 


```{r, engine='bash', count_lines}
#### RULES ####
# Log all kernel messages to the console.
# Logging much else clutters up the screen .
#kern . *
# Log anything (except mail) of level info or higher .
# Don 't log private authentication messages !
/dev/console
* . info;mail. none; authpriv. none; cron . none /var/log/messages
# The authpriv file has restricted access .
authpriv. *
# Log all the mail messages in one place .
mail. *
# Log cron stuff
cron . *

```

## Log file rotation
Logs are rotated by the **logrotate** utility to keep them from filling up the file system containing **/var/log**. When a log file is rotated, it is renamed with an extension indicating the date on which it was rotated:**/var/log/messages8** may become **/var/log/messages-2016-0330**. After a certain number of rotatins, typically after four weeks, the old log file is discarded to free disk space. A cron job runs the logrotate progra daily to see if any logs need to be rotated.

## Monitoring logs with tail
The **tail -f /path_to_file** ouputs the last 10 lines of the file and continues to output new lines as they get written to the monitored file. 

## Finding events with journalctl
The *journalctl* shows the full system journal, starting with the oldest entry.
```{r, engine='bash', count_lines}
[root@serverx -)# journalctl
Feb 14 10: 01:01 server1 run-parts(/etc/cron . hourly} [8678] : starting 0yum-hourly.cron
Feb 14 10: 01:01 server1 run-parts(/etc/cron . hourly} [8682] : finished 0yum-hourly.cron
Feb 14 10:10:01 server1 systemd[1] : Starting Session 725 of user root .
Feb 14 10: 10:01 server1 systemd[1) : Started Session 725 of user root .
```
