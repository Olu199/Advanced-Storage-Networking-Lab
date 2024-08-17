From CPU to storage system:
1. System bus: Through integrated printed circuit that relays communication between CPU cores, and RAM. The information from this bus is transmitted via a chip called as bridge chip.

2. Host I/O bus: receives data from the system bus over the bridge chip. They are a special process and require different kinds of processes.
	1. PCIE is still the most wild spread method of realizing a host bus. They use a either or a combination of software (application drivers alone) and hardware (application and hardware firmware) to enable controller their functions. A wild spread approach implemented is ASIC: Application Specific Intergrated Circut. 
		An example of a host i/o or PCIe is a Network Interface Controller.
1. I/O bus: it is communication between NIC and servers. This communication happens over HBA Host buss adapter.
	Then relays its information over data transmission high ways like:
	- SCSI
	- Fiber Channel
	- Fiber Channel over IP
	- Fiber Channel over Ethernet
	- SCSI is both a protocol and a communication path that has uses parallel data transmission to communicate up to 16 devices irewire
	- Other I/O bus:
		- Serial Storage Architecture(SSA)
		- IEEE 1394 (Apple's Firewire Sony's i.Link)
		- High-Performance Parallel Interface(HIPPI)
		- Advance Technology Attachment(ATA)
		- Serial ATA(SATA)
		- Serial Attached(SCSI)
		- Universal Virtual Bus(USB)
		- Integrated Drive Electronics(IDE)
- Note: All I/O regardless of the high way use SCSI protocol. For example, FC is the highway for interconnected devices yet they all speak the SCSI language

#### I/O Bus
1. SCSI: 
	- Each SCSI bus or port permits only up to 16 ID representing the devices it is connected to
	- It defines both the characteristics of the transmission cable as well as the protocol
	- Connection to other devices is by daisy chining
	- Has a cable length limitation
	- The protocol includes ID information such as:
		- SCSI ID often called Target ID and Initiator ID
		- Logical Unit Numbers (LUNs):  This helps to ovecome the SCSI address number limitations by virtualizing the SCSI ID 
A operating system needs three things for the differentiation of devices:
- Controller ID
- SCSI ID
- LUN