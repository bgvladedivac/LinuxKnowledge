## SCSI
The traditional SCSI hardware setup is a host adapter linked with a chain of devices over an SCSI bus. The host adapter is attached to a computer. The host adapter and devices each have an SCSI ID, and there can be 8/16 per bus. The computer is not directly attached to the device chain, so it must go through the host adapter in order to communicate with disks and other devices.

*SATA* disks also appear on your system as SCSI devices by means of a translation layer in libata library used by the kernel. Some SATA controllers, especially RAID high-performance ones perform this translation in hardware.








