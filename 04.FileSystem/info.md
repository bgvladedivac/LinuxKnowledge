## Confusion
The term filesystem has two somewhat different meanings, both of which are commonly used. This can be confusing to novices, but after a while the meaning is usually clear from the context.

One meaning is the entire hierarchy of directories (also referred to as the directory tree) that is used to organize files on a computer system. On Linux and Unix, the directories start with the root directory (designated by a forward slash), which contains a series of subdirectories, each of which, in turn, contains further subdirectories, etc.

The second meaning is the type of filesystem, that is, how the storage of data (i.e., files, folders, etc.) is organized on a computer disk (hard disk, floppy disk, CDROM, etc.) or on a partition on a hard disk. Each type of filesystem has its own set of rules for controlling the allocation of disk space to files and for associating data about each file (referred to as meta data) with that file, such as its filename, the directory in which it is located, its permissions and its creation date.

## Summary
A file system is allocated on the so called *block device*(disks, partitions, virtual-block devices created by LVM..). A file system is an organized collection of files and directories. Linux implements a variety of file systems, including the traditional *ext* family. Each file in a file system has an entry in the file system's *i-node table*. This entry contains information about the file, things like type, size, link count, ownership ... (metadata).

<br />
Linux provides a range of **journaling** file systems, including *Reiserfs, ext3, ext4, XFS, JFS and Btrfs*. A *journaling* file system records metadata updates to a log file before the actual file updates are performed. This means that in the event of a system crash, the log file can be replayed to quickly restore the file system to a consistent state. The *key benefit* of journaling file systems is that they avoid the long *fsck* required by conventional UNIX file systems after a system crash.

<br /> All file systems on a Linux node are **mounted** under a single directory tree. The location at which a file system is mounted in the directory tree is called its **mount point**. 

## File-system structure
The basic unit for allocating space in a file system is a *logical block*, which is some multiple of contiguous physical blocks on the disk device on which the file system resides. Example, the logical block size on *ext2* is 1024, 2048 or 4096 bytes. The logical block size is specified as an argument of the *mkfs* command used o build the file system. <br />

A file system contains the following parts: <br />
1. *Boot block* is the first block in a file system. The boot block is not used by the file system, rather it contains information used to boot the OS. Although only one boot block is needed by the OS, all file systems have a boot block(most of which are unused).<br />
2. *Superblock* is a single block, which contains parameter information about the file system(the size of the i-node table, the size of the logical blocks in the file system, the size of the file system in logical blocks ...). <br />
3. *I-node table*, each file/directory in the file system has a unique entry in the i-noe table. This entry records different information about the file. <br />








