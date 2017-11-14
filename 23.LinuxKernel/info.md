## Kernel modules
Kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand. They extend the functionality of the kernel without the need to reboot the system. Modules are stored in */usr/lib/modules/kernel_release*. You can use the command *uname -r* to get your current kernel release version. To show what kernel modules are currently loaded:

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


