## Description
FC presents block devices similiar to iSCSI. I/O operations are carried out over a netwok, using a block access protocol. In FC, remote bloks are accessed by encapsulating SCSI commands and data into FC frames. FC is commonly deployed in the majority of mission-critical environments. 
## Implementation
Requires a dedicated host bus adapter(HBA)(typically two, for redundancy and multipathing).
## Performance
FC can run on 1/2/4/8/16 GB HBAs. The protocol affects a host's CPU the least, because HBAs handle most of the processins.
## Advantages
Well known and well-understood. Very mature and trusted. 
## Disadvantages
Dedicated HBA, FC switch anf FC-capable stroage array, which makes an FC implementation somewhat more expensive.
