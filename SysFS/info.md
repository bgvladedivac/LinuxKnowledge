## SysFs
The **/sys** filesystem (sysfs) contains files that provide information about devices: whether it's powered on, the vendor name and model, what bus the device is plugged into, etc. It's of interest to applications that manage devices. Modern Linux distributions include a **/sys**  directory as a virtual filesystem (sysfs, comparable to **/proc**, which is a procfs), which stores and allows modification of the devices connected to the system, whereas many traditional UNIX and Unix-like operating systems use **/sys** as a symbolic link to the kernel source tree.
<br />
Some of the SysFs directories:


**bus** contains flat directory layout of the various bus types in the kernel. Each bus's directory contains two subdirectories: devices and drivers.<br />

**dev** contains the directories char/ and block/. Inside these two directories there are symlinks named <major>:<minior>. These symlinks point to the sysfs directory for the given device. /sys/dev provides a quick way to lookup the sysfs interface for a device from the result of a stat(2) operation. <br />
**devices** contains a filesystem representation of the device tree. It mapsdirectly to the internal kernel device tree, which is a hierarchy of struct device.<br />
**net**  **block** **fs** **class**
<br /> 

Setting the brightness of a laptop monitor could be achieved from sys:
```{r, engine='bash', count_lines}
echo N > /sys/class/backlight/acpi_video0/brightness

```
