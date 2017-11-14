## System virtualization
**KVM**(Kernel-based Virtual Machine) is a full virtualization solution built into the Linux kernel. The KVM hypervisor is managed with the *libvirt API* and *virt-manager*/*virsh*. The KVM hypervisor requires either an Interl processor with the Intel VT-x and Intel 64 extensions for x86-based systems, or an AMD processor with the AMD-V and the AMD64 extensions. To verify that the host system hardware suppports the correct extensions, view **/proc/cpuinfo**.
```{r, engine='bash', count_lines}
[root@serverX -]# grep - - color -E "vmx l svm" /proc/cpuinfo
```
Building a virtual host requires the qemu-kvm and qemu-img packages at minimum, to provide the user-leve l KVM emulator and disk image manager.
```{r, engine='bash', count_lines}
[root@serverX -]# yum install qemu-kvm qemu-img
```
Additional virtualization management packages are also recommended:<br />
• *python-virtinst* - provides the virt-install command for creating virtual machines.<br />
• *libvirt* - provides the host and server libraries for interacting with hypervisors and host systems.<br />
• *libvirt-python* - contains a module to permit Python applications to use the libvirt API.<br />
• *virt-manager* - provides the Virtual Machine Manager graphical tool for administering VMs, using the libvirt-client library as the management API.<br />
• *libvirt-client* - provides the client APls and libraries for accessing libvirt servers, including the virsh command-line tool to manage and control VMs.

## Controlling virtual machines
The libvirt package is a hypervisor-independent virtualization API to securely control virtual machines by providing the ability to provision, create, modify, monitor ... virtual machines on a single host. The libvirt package provides APls to enumerate, monitor,
and use the resources available on the managed host, including CPUs, memory, storage, and networking. The fundamentals tools included: <br />

*virsh* - an alternative to the graphical virt-manager application. The virsh command is ideal for scripting virtualization administration. <br />

*virt-manager* - a graphical desktop tool allowing access to guest consoles and is used to perform virtual machine creation, migration, configuration, and administrative tasks. Both local and remote hypervisors can be managed through a single interface.

The virsh command-line tool provides the same functionality as virtual manager.
```{r, engine='bash', count_lines}
[root@serverX -]# virsh list
Id Name State
1 desktop
2 server
running
running
[root@serverX -]# virsh destroy server
[root@serverX -]# virsh list - - all
Id Name State
1 desktop
- server
running
shut off
[root@serverX -]# virsh start server
[root@serverX -]# virsh list
Id Name State
1 desktop
2 server
running
running 
```
