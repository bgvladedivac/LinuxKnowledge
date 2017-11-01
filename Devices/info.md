## Device special files/API
A device special file corresponds to a device on the system. Within the kernel, each device type has a corresponding device driver(a unit of kernel code), which handles all I/O requests for the device.  The API provided by device drivers is fixed,  to the system calls **open()**, **close()**, **read()**, **write()**, **mmap()**, and **ioctl()**.

The model that each device driver provides a consistent interface, hiding the differences in operation of individual devices, allows for universality of I/O.

## Main distinguishment Character vs. block devices

## Character devices
Those for which no buffering is performed. They handle data on a character-by-character basis. Terminals and keyboards are exampples of character devices. **Character devices** are read from and written to with two function: foo_read() and foo_write().

```{r, engine='bash', count_lines}
[devil@client ~]$ ls -l /dev/tty0
crw--w----. 1 root tty 4, 0 Nov  1 10:35 /dev/tty0
```

## Block devices
**Block devices** handle data a block at a time. The size of a block depeneds on the type of device, but is typically some multiple of 512 bytes. Examples of block devices include disks, partitions and virtual-block devices(created by LVM). **Filesystems can only be mounted if they are on block device.** . **Block devices** have a function which has historically been called the ``strategy routine.'' Reads and writes are done through the buffer cache mechanism by the generic functions bread(), breada(), and bwrite(). A request may be asyncronous: breada() can request the strategy routine to schedule reads that have not been asked for, and to do it asyncronously, in the background, in the hopes that they will be needed later.

```{r, engine='bash', count_lines}
[root@client devil]# ls -l /dev/sda
brw-rw----. 1 root disk 8, 0 Nov  1 10:35 /dev/sda
[root@client devil]# ls -l /dev/sda1
brw-rw----. 1 root disk 8, 1 Nov  1 10:35 /dev/sda1
[root@client devil]# ls -l /dev/mapper/centos-
centos-root  centos-swap  
[root@client devil]# ls -l /dev/mapper/centos-root 
lrwxrwxrwx. 1 root root 7 Nov  1 10:35 /dev/mapper/centos-root -> ../dm-0
[root@client devil]# ls -l /dev/dm-0
brw-rw----. 1 root disk 253, 0 Nov  1 10:35 /dev/dm-0
```




