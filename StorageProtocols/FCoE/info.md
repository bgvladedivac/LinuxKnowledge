## Description
FCoE presents block devices, with I/O operations carried out over a network using a block access protocol. In this protocol, SCSI commands and data are encapsulated into Ethernet frames. 
## Impelementation
Using **Hardware converged network adapter(CNA)** or **Nework adapter with FCoE capabilities**, using software FCoE intiator.
## Performance
The protocol **requires 10Gb Ethernet** and the so called **jumbo frames**, because FC payloads are 2.2K in size and cannot be fragmenented. FCoE is SCSI over ethernet, not IP.
## Advatanges
Enable consolidation of storage and other traffic onto the same network via CNA.
## Disadvantages
Requires 10Gb network intrastructure, which can be expensive. 
