## Confusion
The term filesystem has two somewhat different meanings, both of which are commonly used. This can be confusing to novices, but after a while the meaning is usually clear from the context.

One meaning is the entire hierarchy of directories (also referred to as the directory tree) that is used to organize files on a computer system. On Linux and Unix, the directories start with the root directory (designated by a forward slash), which contains a series of subdirectories, each of which, in turn, contains further subdirectories, etc.

The second meaning is the type of filesystem, that is, how the storage of data (i.e., files, folders, etc.) is organized on a computer disk (hard disk, floppy disk, CDROM, etc.) or on a partition on a hard disk. Each type of filesystem has its own set of rules for controlling the allocation of disk space to files and for associating data about each file (referred to as meta data) with that file, such as its filename, the directory in which it is located, its permissions and its creation date.

## Summary
A file system is allocated on the so called **block device**(disks, partitions, virtual-block devices created by LVM..). A file system is an organized collection of files and directories. Linux implements a variety of file systems, including the traditional *ext* family. Each file in a file system has an entry in the file system's *i-node table*. This entry contains information about the file, things like type, size, link count, ownership ... (metadata).

<br />
Linux provides a range of *journaling* file systems, including *Reiserfs*, *ext3*, *ext4*, *XFS*, *JFS* and *Btrfs*. A *journaling* file system records metadata updates to a log file before the actual file updates are performed. This means that in the event of a system crash, the log file can be replayed to quickly restore the file system to a consistent state. The *key benefit* of journaling file systems is that they avoid the long *fsck* required by conventional UNIX file systems after a system crash.

<br /> All file systems on a Linux node are **mounted** under a single directory tree. The location at which a file system is mounted in the directory tree is called its **mount point**. 

## File-system structure
The basic unit for allocating space in a file system is a *logical block*, which is some multiple of contiguous physical blocks on the disk device on which the file system resides. Example, the logical block size on *ext2* is 1024, 2048 or 4096 bytes. The logical block size is specified as an argument of the *mkfs* command used o build the file system. <br />

A file system contains the following parts: <br />
1. **Boot block** is the first block in a file system. The boot block is not used by the file system, rather it contains information used to boot the OS. Although only one boot block is needed by the OS, all file systems have a boot block(most of which are unused).<br />
2. **Superblock** is a single block, which contains parameter information about the file system(the size of the i-node table, the size of the logical blocks in the file system, the size of the file system in logical blocks ...). <br />
3. **I-node table**, each file/directory in the file system has a unique entry in the i-noe table. This entry records different information about the file. <br />

## I-nodes
A file system's i-node table contains one i-node(short for index node) for each file residing in the file system. The i-node number of a file is the first field by the **ls -li** command. The informaion maintained in an i-node includes the following:<br />
1. File type (regular file, symbolik link, character device, directory ...)<br />
2. Owner<br />
3. Group<br />
4. Access permissions<br />
5. Three timestamps: time of last access to the file(*ls -lu*), time of last modification of the file(*ls -l*) and time of last statues change(*ls -lc*).<br />
6. Number of hard links to the file. If the number is 1, once you delete the file, it can't be restored.<br />
7. Number of blocks actually allocated to the file, measured in units of 512 bytes blocks.<br />

## The Virtual File System(VFS)
Each of the file systems differ in the details of their implementations. For example, the way in which the blocks of a file are allocated. If every program that worked with the files needed to understand the specific details of each file system, the task of writing programs that worked with all of the different file systems would be impossibble. The **VFS** is a kernel feature that resolves this problem by creating an abstraction layer for file-system operations. VFS defines an *interface* for file-system operations. All programs that work with files specify their operations in terms of this interface. Each file system provides an implementation for the VFS interface. Under this scheme, programs need to understand only the VFS interface and can ignore details of inidividual file-system implementations.

## Journaling file systems
The ext2 is an example of traditional UNIX file system which suffers from a classic limitation, after a system crash file-system consistency check(*fsck*) must be performed on reboot in order to ensure the integrity of the file system. The issue is that a consistency check requires examining the entire file system. On a large one, this may require several hours. Journaling file systems eliminate the need for lengthy file-system consistency checks after a system crash. A journaling file system logs(*journals*) all metadata updates to a specific file, before they are actually carried out. In the event of a system crash, the log can be used to rapidly redo any incomplete updates and bring the file system back to a consistent state.

## File system locations
* A list of currently mounted file systems can be read from the Linux-specific */proc/mounts* virtual file. */proc/mounts* is an interface to kernel data structures, so it always contains accurate information about mounted file systems.<br />
* The */etc/fstab* file maintained manually contains descriptions of all the available file systems and is used by *mount* and *fsck*.
A line from the file:
```{r, engine='bash', count_lines}
/dev/sda1 /boot ext4 defaults 0 1
```
The line contains six fields:<br />
1. The name of the mounted device. Be sure the name to be unique. If the device is LVM based, you are safety. For all other devices, except */dev/sda1* use their unique block id.<br />
2. The mount point for the device. <br />
3. The file system type.<br />
4. Mount options.<br />
5. A number used to control the operation of file system backups by *dump*. This field and the next one are used only in the */etc/fstab* file. 
6. A number used to control the order in which *fsck* checks file systems at system boot time.

## Unmounting a file system
It is not possible to unmount a file system that is busy(there are open files on the file system or a process's current working direcotry is somewhere in the file system). The **lsof** lists all open files and the process accessing them in the provided directory.

## A virtual memory file system
Linux also suports *virtual file systems* that reside in memory. To applications, they look like any other file system. However, one important difference is that file operations are much faster, since no disk access is involved.

## Examining file systems
To get an overview about the file system mount points and the amount of free space available, run **df**. To improve readability of the output sizes, there are two different *human-readable* options *-h* and *-H*. <br />

-h will report in decimal value system(KiB, MiB, GiB), while -H will report in decimal(LB, MB, GB).

```{r, engine='bash', count_lines}
[vieri@localhost ~]& df -h
Filesystem Size Used Avail Use% Mounted on
/dev/vda1 6.0G 3.9G 2.2G 65% I
devtmpfs 929M 0 929M 0% /dev
tmpfs 937M 80K 937M 1% /dev/shm
```
For more detailed information about the space used by a certain directory tree, there is the **du** command.
