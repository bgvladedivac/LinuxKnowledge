## Kernel Responsibility
It's responsible for enabling multiple applications to effectively share the hardware by controlling access to CPU, memory, disk I/O, and networking. 

## Location
The kernel file is stored in your /boot folder and is called *vmlinuz-version*. 

## Initial ramdisk
*initrd* (initial ramdisk) is a scheme for loading a temporary root file system into memory, which may be used as part of the Linux startup process. initrd and *initramfs* refer to two different methods of achieving this. Both are commonly used to make preparations before the real root file system can be mounted.

Many Linux distributions ship a single, generer Linux kernel image. The device drivers for this generic kernel image are included as *loadable kernel modules*, because statically compiling many drivers into one kernel causes the kernel image to be much larger, perhaps too large on computers with limited memory. This then raises the problem of detecting and loading the modules necessary to mount the root file system at boot time(which can be allocated on RAID, NFS, encryted partition ...) . To avoid handling so many special cases into the kernel, an initial boot stage with a temporary root file-system is used. This root file-system can contain user-space helpers which do the hardware detection, module loading and device discovery necessary to get the real root file-system mounted.

The bootloader will load the kernel and initial root file system image into memory. 

Almost all Linux nodes uses initramfs. The only purpose of an initramfs is to mount the root filesystem. It is bundled into a single cpio archive and compressed with one of several compression algorithms. The location of the initramfs is */boot*.

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


