## Description
iSCSI presents bloc devices to host. Rather than accessing blocks from a local disk, I/O operations are carried out over a networking uing a block access protocol. Remote blocks are accessed by encapsulating SCSI commands and data into TCP/IP packets.
## Implementation
Network adapter with iSCSI capabilities, using sotware iSCSI initiator. 
## Performance
Can run over a 1 or a 10 Gb, TCP/IP network. 
## Advantages
No additional hardware is necessary.
## Disadvantages
Inability to route with iSCSI binding implemented. Possible security issues because there is no built-in encryption, so care must be taken to isolate traffic(e.g, VLANS).
