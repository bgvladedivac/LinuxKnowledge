## Description
FC presents block devices similiar to iSCSI. I/O operations are carried out over a netwok, using a block access protocol. In FC, remote bloks are accessed by encapsulating SCSI commands and data into FC frames. FC is commonly deployed in the majority of mission-critical environments. 
## Implementation
Requires a dedicated host bus adapter(HBA)(typically two, for redundancy and multipathing). Each HBA has a unique World Wide Name(WWN), which is similiar to an Ethernet MAC address. The connection from the server through HBA to the storage controller is referred as a path. When multiple paths exists to a storage device(LUN) on a storage subsystem, it is referred as **multipath** connectivity. A SCSI device is configured for a LUN seen on each path, i.f, if a LUN has 4 paths, then one will see four SCSI devices getting configured for the same device.

## Performance
FC can run on 1/2/4/8/16 GB HBAs. The protocol affects a host's CPU the least, because HBAs handle most of the processins.
## Advantages
Well known and well-understood. Very mature and trusted. 
## Disadvantages
Dedicated HBA, FC switch anf FC-capable stroage array, which makes an FC implementation somewhat more expensive.
