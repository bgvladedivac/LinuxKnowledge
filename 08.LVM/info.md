## LVM(Logical Volumen Manager)
LVM gives you the main benefit ot controlling your storage in a more flexible way. It also supports snapshots and mooving around physical volumes with the *pvmove* command. The foundation of LVM is the device mapper for mapping physical block devices onto higher-level virtual block devices. Once you end up with **logical volumes** you could allocate file systems on them and resize them if necessary. 

**Logical volumes** and logical volume management make it easier to manage disk space. If a LVM hosted file system needs more space, it can be allocated to its logical volume from the free space in its **volume group** and the file system can be resized. If a disk starts to fail, a replacement disk can be registered as a physical volume with the volume group and the logical volume's extents can be migrated to the new disk.

## LVM Definitions
• Physical devices are the storage devices used to persist data stored in a logical volume. These are block devices and could be disk partitions, whole disks, RAID arrays, or SAN disks. A device must be initialized as an LVM physical volume in order to be used with LVM. The entire "device" will be used as a physical volume.

• **Physical volumes (PV)** are used to label physical devices for use in volume groups(**container**). LVM automatically segments PVs i nto **physical extents (PE)** which these are small chunks of data that act as the smal lest storage block on a PV.

• **Volume groups (VG)** are storage pools made up of one or more physical volumes. A PV can only be allocated to a single VG. A VG can consist of unused space and any number of logical volumes.

• **Logical volumes (LV)** are created from free physical extents in a volume group and provide the "storage" device used by applications, users, and the operating system. LVs are a collection of logical extents (LE), which map to physical extents, the smallest storage chunk of a PV. By default, each LE will map to one PE. Setting specific LV options will change this mapping.

## Normal workflow for creating Logical volume
```{r, engine='bash', count_lines}
[ root@server -]# pvcreate /dev/sdb /dev/sdc
[ root@server -]# vgcreate alpha /dev/sdb /dev/sdc 
[ root@server -]# lvcreate -n hercules -L 2G alpha 
[ root@server -]# mkfs -t xfs /dev/alpha/hercules 

[root@serverx -]# mkdir /mnt/hercules
• Add an entry to the /etc/fs t ab file:

/dev/alpha/hercules /mnt/hercules xfs defaults 1 2
[ root@servex -]# mount -a 
```

## Reviewing LVM status information 
```{r, engine='bash', count_lines}
[ root@server -]# pvdisplay | pvs
[ root@server -]# vgdisplay | vgs
[ root@server -]# lvdisplay | lvs
```
