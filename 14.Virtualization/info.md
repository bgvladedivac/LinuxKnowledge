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
• python-virtinst-Provides the virt-install command for creating virtual machines.<br />
• libvirt-Provides the host and server libraries for interacting with hypervisors and host systems.<br />
• libvirt-python-Contains a module to permit Python applications to use the libvirt API.<br />
• virt-manager- Provides the Virtual Machine Manager graphical tool for administering VMs, using the libvirt-client library as the management API.<br />
• libvirt-client-Provides the client APls and libraries for accessing libvirt servers, including the virsh command-line tool to manage and control VMs.
