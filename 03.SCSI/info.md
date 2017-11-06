## SCSI
The traditional SCSI hardware setup is a host adapter linked with a chain of devices over an SCSI bus. The host adapter is attached to a computer. The host adapter and devices each have an SCSI ID, and there can be 8/16 per bus. The computer is not directly attached to the device chain, so it must go through the host adapter in order to communicate with disks and other devices.

*SATA* disks also appear on your system as SCSI devices by means of a translation layer in libata library used by the kernel. Some SATA controllers, especially RAID high-performance ones perform this translation in hardware.
```{r, engine='bash', count_lines}
$ lsscsi
[0:0:8:0]    disk    FUJITSU  MAM3184MP        0105  /dev/sda
[2:0:0:0]    cd      CREATIVE CD5233E          1.00  /dev/scd0
[3:0:5:0]    tape    HP       C5713A           H910  /dev/st0
[3:0:5:1]    mediumx HP       C5713A           H910  -
[4:0:0:0]    disk    Linux    scsi_debug       0004  /dev/sdb
```

The first entry on each line is the SCSI host adapter, SCSI bus number, SCSI ID and the LUN(logical unit number, a further subdivision a device). When there are multiple SCSI devices their entries are sorted in ascending tuple order. The next column is the SCSI peripheral type. Then follows the vendor name, the model name and the revision string. The last entry is the primary device node name. The "primary" device node name is associated with the upper level SCSI driver that "owns" the device. Examples of upper level SCSI drivers are sd (for disks), sr (for optical drives whose devices are often named /dev/scd<n> ) and st (for tapes). Some SCSI devices have peripheral types that either don't have upper level drivers to control them, or the associated driver module is not loaded. Such devices have '-' given for their device node name. All SCSI devices can be accessed via their corresponding scsi generic (sg) device node name (e.g. /dev/sg<n> ) which can be seen by adding a '--generic' option to the above lsscsi invocation.

By adding the '--size' option ('-s' in its short form) the size of disks is shown to the right of each line:

```{r, engine='bash', count_lines}
# lsscsi -s 
[0:0:0:0]    cd/dvd  PIONEER  DVD-RW  DVR-212D 1.22  /dev/sr0        - 
[1:0:0:0]    disk    ATA      ST3320620AS      3.AA  /dev/sda    320GB 
[6:0:0:0]    disk    SEAGATE  ST32000444SS     0006  /dev/sdb   2.00TB 
```

The device node major and minor numbers can also be output with the '-d' option:
```{r, engine='bash', count_lines}
$ lsscsi -d
[0:0:1:0]    disk    FUJITSU  MAM3184MP        0105  /dev/sda[8:0]
```
## Disks
Most hard disks attached to current Linux systems correspond to device names with an *sd* prefix, such as */dev/sda*. These devices represent entire disks, the kernel makes seperate device files, such as */dev/sda1* for the partitions on a disk. The *sd* portion of the name stands for *SCSI disk*. SCSI was originally developed as hardware and protocol standard for communication between devices such as disks and other peripherals. Although traditional SCSI hardware is not used in most modern machines, the SCSI protocols is everywhere due to its adaptability. 

## Rescanning your SCSI bus to see new storage
To make the operating system aware of the new storage device, or path to an existing device, the recommended command to use is:
```{r, engine='bash', count_lines}
$ lsscsi -d
$ echo "c t l" >  /sys/class/scsi_host/hosth/scan
```
In the command, h is the HBA number, c is the channel on the HBA, t is the SCSI target ID, and l is the LUN.

