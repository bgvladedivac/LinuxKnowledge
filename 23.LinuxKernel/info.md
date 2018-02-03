## Kernel Responsibility
It's responsible for enabling multiple applications to effectively share the hardware by controlling access to CPU, memory, disk I/O, and networking. 

## Location
The kernel file is stored in your /boot folder and is called *vmlinuz-version*. 

## Version
The easist way to find the kernel version is to use the *uname -r*.
```{r, engine='bash', count_lines}
[root@localhost grub2]# uname -r
3.10.0-693.el7.x86_64
```
## Kernel modules
Kernel modules/Dynamically loadable kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand. They extend the functionality of the kernel without the need to reboot the system. Modules are stored in */usr/lib/modules/kernel_release*. You can use the command *uname -r* to get your current kernel release version. To show what kernel modules are currently loaded:

```{r, engine='bash', count_lines}
[root@localhost 3.10.0-693.el7.x86_64]# lsmod | more
Module                  Size  Used by
ip6t_rpfilter          12595  1
ipt_REJECT             12541  2
nf_reject_ipv4         13373  1 ipt_REJECT
ip6t_REJECT            12625  2
nf_reject_ipv6         13717  1 ip6t_REJECT
xt_conntrack           12760  11
```

To get information about a module, use the *modinfo mod_name*:
```{r, engine='bash', count_lines}
[root@localhost 3.10.0-693.el7.x86_64]# modinfo xt_conntrack
filename:       /lib/modules/3.10.0-693.el7.x86_64/kernel/net/netfilter/xt_conntrack.ko.xz
alias:          ip6t_conntrack
alias:          ipt_conntrack
description:    Xtables: connection tracking state match
author:         Jan Engelhardt <jengelh@medozas.de>
author:         Marc Boucher <marc@mbsi.ca>
license:        GPL
rhelversion:    7.4
srcversion:     FE59F0C3EDC3B3EAC3BA22E
depends:        nf_conntrack
intree:         Y
vermagic:       3.10.0-693.el7.x86_64 SMP mod_unload modversions
signer:         CentOS Linux kernel signing key
sig_key:        DA:18:7D:CA:7D:BE:53:AB:05:BD:13:BD:0C:4E:21:F4:22:B6:A4:9C
sig_hashalgo:   sha256

```
To display the comprehensive configuration of all the modules *$ modprobe -c | less*, to display the configuration of a particular module *modeprobe -c | grep module_name*. List the dependencies of a module *modprobe -c | grep mod_name*.

A standard directory in Linux where driver files are stored is */lib/modules/$Kernel_version/kernel/drivers/*.  Most files are *XZ compressed data* ending in *.ko* extensions. Ko stands for kernel object.


To get information about a module, use the *modinfo mod_name*:
```{r, engine='bash', count_lines}
[root@localhost 3.10.0-693.el7.x86_64]# modinfo xt_conntrack
filename:       /lib/modules/3.10.0-693.el7.x86_64/kernel/net/netfilter/xt_conntrack.ko.xz
alias:          ip6t_conntrack
alias:          ipt_conntrack
description:    Xtables: connection tracking state match
author:         Jan Engelhardt <jengelh@medozas.de>
author:         Marc Boucher <marc@mbsi.ca>
license:        GPL
rhelversion:    7.4
srcversion:     FE59F0C3EDC3B3EAC3BA22E
depends:        nf_conntrack
intree:         Y
vermagic:       3.10.0-693.el7.x86_64 SMP mod_unload modversions
signer:         CentOS Linux kernel signing key
sig_key:        DA:18:7D:CA:7D:BE:53:AB:05:BD:13:BD:0C:4E:21:F4:22:B6:A4:9C
sig_hashalgo:   sha256

```

Each module is loaded with the **module_init**, when removed the **module_exit** is used. Basic C file for a module.

```{r, engine='bash', count_lines}
#include <linux/module.h>
#include <linux/sched.h>
#include <linux/kernel.h>

int my_init_module (void)
{
	printk("The module is now loaded\n");
	return 0;
}

module_init (my_init_module);

void my_cleanup_module (void)
{
	printk("The module is now unloaded\n");
}

module_exit (my_cleanup_module)

```

After initialization of the module search in **dmesg** for the output.

## Kernel Data Structures

The operating system must keep a lot of information about the current state of the system. As things happen within the system these data structures must be changed to reflect the current reality. For example, a new process might be created when a user logs onto the system. The kernel must create a data structure representing the new process and link it with the data structures representing all of the other processes in the system.

Mostly these data structures exist in physical memory and are accessible only by the kernel and its subsystems. Data structures contain data and pointers; addresses of other data structures or the addresses of routines. Taken all together, the data structures used by the Linux kernel can look very confusing. Every data structure has a purpose and although some are used by several kernel subsystems, they are more simple than they appear at first sight.

**Linked Lists**

Linux uses a number of software engineering techniques to link together its data structures. On a lot of occasions it uses linked or chained data structures. If each data structure describes a single instance or occurance of something, for example a process or a network device, the kernel must be able to find all of the instances. In a linked list a root pointer contains the address of the first data structure, or element, in the list and each data structure contains a pointer to the next element in the list. The last element's next pointer would be 0 or NULL to show that it is the end of the list. In a doubly linked list each element contains both a pointer to the next element in the list but also a pointer to the previous element in the list. Using doubly linked lists makes it easier to add or remove elements from the middle of list although you do need more memory accesses. 

**Hash Tables**
Linked lists are handy ways of tying data structures together but navigating linked lists can be inefficient. If you were searching for a particular element, you might easily have to look at the whole list before you find the one that you need. Linux uses another technique, hashing to get around this restriction. A hash table is an array or vector of pointers. An array, or vector, is simply a set of things coming one after another in memory. A bookshelf could be said to be an array of books. Arrays are accessed by an index, the index is an offset into the array. Taking the bookshelf analogy a little further, you could describe each book by its position on the shelf; you might ask for the 5th book.

A hash table is an array of pointers to data structures and its index is derived from information in those data structures. If you had data structures describing the population of a village then you could use a person's age as an index. To find a particular person's data you could use their age as an index into the population hash table and then follow the pointer to the data structure containing the person's details. Unfortunately many people in the village are likely to have the same age and so the hash table pointer becomes a pointer to a chain or list of data structures each describing people of the same age. However, searching these shorter chains is still faster than searching all of the data structures.

As a hash table speeds up access to commonly used data structures, Linux often uses hash tables to implement caches. Caches are handy information that needs to be accessed quickly and are usually a subset of the full set of information available. Data structures are put into a cache and kept there because the kernel often accesses them. There is a drawback to caches in that they are more complex to use and maintain than simple linked lists or hash tables. If the data structure can be found in the cache (this is known as a cache hit, then all well and good. If it cannot then all of the relevant data structures must be searched and, if the data structure exists at all, it must be added into the cache. In adding new data structures into the cache an old cache entry may need discarding. Linux must decide which one to discard, the danger being that the discarded data structure may be the next one that Linux needs.

**Abstract Interfaces**

The Linux kernel often abstracts its interfaces. An interface is a collection of routines and data structures which operate in a particular way. For example all network device drivers have to provide certain routines in which particular data structures are operated on. This way there can be generic layers of code using the services (interfaces) of lower layers of specific code. The network layer is generic and it is supported by device specific code that conforms to a standard interface.

Often these lower layers register themselves with the upper layer at boot time. This registration usually involves adding a data structure to a linked list. For example each filesystem built into the kernel registers itself with the kernel at boot time or, if you are using modules, when the filesystem is first used. You can see which filesystems have registered themselves by looking at the file /proc/filesystems. The registration data structure often includes pointers to functions. These are the addresses of software functions that perform particular tasks. Again, using filesystem registration as an example, the data structure that each filesystem passes to the Linux kernel as it registers includes the address of a filesystem specfic routine which must be called whenever that filesystem is mounted.


## Initial ramdisk
*initrd* (initial ramdisk) is a scheme for loading a temporary root file system into memory, which may be used as part of the Linux startup process. initrd and *initramfs* refer to two different methods of achieving this. Both are commonly used to make preparations before the real root file system can be mounted.

Many Linux distributions ship a single, generer Linux kernel image. The device drivers for this generic kernel image are included as *loadable kernel modules*, because statically compiling many drivers into one kernel causes the kernel image to be much larger, perhaps too large on computers with limited memory. This then raises the problem of detecting and loading the modules necessary to mount the root file system at boot time(which can be allocated on RAID, NFS, encryted partition ...) . To avoid handling so many special cases into the kernel, an initial boot stage with a temporary root file-system is used. This root file-system can contain user-space helpers which do the hardware detection, module loading and device discovery necessary to get the real root file-system mounted.

The bootloader will load the kernel and initial root file system image into memory. 

Almost all Linux nodes uses initramfs. The only purpose of an initramfs is to mount the root filesystem. It is bundled into a single cpio archive and compressed with one of several compression algorithms. The location of the initramfs is */boot*.


## Fetching Kernel Source on CentOS

```{r, engine='bash', count_lines}
yumdownloader --source kernel # fetch the source in rpm
rpm -iv kernel*.rpm
cd ~/rpmbuild/SPECS
rpmbuild -bp kernel.spec 2> /tmp/modules # you might need to install some dependencies
for i in $(cat /tmp/modules);do yum -y install $i;done
```

All configuration choices are stored in .config.
Do not edit it directly, use the make file.

```
make menuconfig 
make xconfig
```






