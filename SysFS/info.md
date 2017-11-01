## SysFs

The sysfs directory arrangement exposes the relationship of kernel data structures. The top level sysfs directory looks like:

**block** <br />
**bus**: contains flat directory layout of the various bus types in the kernel. Each bus's directory contains two subdirectories: devices and drivers.<br />
**class** <br />
**dev**: contains the directories char/ and block/. Inside these two directories there are symlinks named <major>:<minior>. These symlinks point to the sysfs directory for the given device. /sys/dev provides a quick way to lookup the sysfs interface for a device from the result of a stat(2) operation. <br />
**devices**: contains a filesystem representation of the device tree. It mapsdirectly to the internal kernel device tree, which is a hierarchy of struct device.<br />
**net/**<br />
**fs/**<br /> 

```{r, engine='bash', count_lines}
echo N > /sys/class/backlight/acpi_video0/brightness
```
