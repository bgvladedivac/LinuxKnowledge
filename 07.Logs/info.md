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

The **#### RU LES ####** section of */etc/rsyslog.conf* contains directives that define where log messages are saved. The left side of each line indicates the facility and severity of the log message the directive matches. The rsyslog.conf file can contain the character * as a wild ca rd in the facility and severity field, where it either stands for all facilities or all severities. The right
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
