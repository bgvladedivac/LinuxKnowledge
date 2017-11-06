## Udev
The Linux kernel can send notifications to a user-space process(udevd) upon detecting a new device on the system. The user-space process on the other end examines the new device's characteristics, creates a device file and then performs device initialization. The udevd daemon operates as follows: <br />
1. The kernel sends *udevd* a notification event called *uevent*.<br />
2. *Udevd* loads all of the attributes in the uevent.<br />
3. *Udevd* parses its rules and it takes actions or sets attributes based on those rules.<br />

An incoming uevent that udevd receives from the kernel might look like this:
```{r, engine='bash', count_lines}
ACTION=change
DEVNAME=sdc
DEVPATH=/devices/pci0000:00/0000:00:1a.0/usb1 ...
DEVTYPE=disk
MAJOR=8
...
```

4. After receiving the uevent, udevd knows the sysfs device path and a number of other attributes associated with the properties and is ready to start processing *rules*. The rules files are in */lib/udev/rules.d* and */etc/udev/rules.d*. The rules in the first directory are default ones, while in the second ones override them.

