
## Storage Tutorial

### Service Delivery Models and Architecture

#### Server Centric Model
- **Characteristics**:
  - Uses iSCSI and Shared LAN.
  - Prioritizes availability, scalability, and security.
  - Focuses on data speed.
  - Manages disk resource sharing and allocation.
  - Often results in distributed, disjointed, and vulnerable setups.
  - Associated with higher costs and management complexities.

#### Storage Centric Model
- **Characteristics**:
  - Uses independent networks.
  - Emphasizes availability, scalability, and security.
  - Focuses on data speed.
  - Manages disk resource sharing and allocation more effectively.
  - Offers a more cohesive and secure architecture.
  - Potentially lower costs and easier management.
  - **Key Features**:
    - **High Availability**: Ensures that data is accessible and systems remain operational.
    - **High Performance**: Delivers fast data access and processing speeds.
    - **Instant Copy**: Provides the capability to quickly create copies of data.
    - **Remote Mirror Availability**: Allows for data to be mirrored to remote locations for added redundancy and disaster recovery.

### Architecture of Intelligent Disk Subsystems: AKA Storage Centric Model

#### Connections to the Server and Underlying Disk

**Method 1**:
- **Connection by Small Computer System Interface (SCSI)**:
  - Uses Fiber Channel or Internet connections by Small Computer System Interface (iSCSI).

**Method 2**:
- **Ports**: Utilizes various ports for connections.
- **Internal I/O Channels**: Leverages internal I/O channels for data transfer.

---

This revision now includes the architecture and connection methods for the Storage Centric Model as highlighted in the third image, providing a more detailed and accurate representation of the storage tutorial.
## RAID Types

### RAID 0
- **Data Striping:** Data is striped across multiple disks in blocks.
- **Virtual Hard Disk:** A virtual hard disk is presented to the server, which then distributes information to the physical disks based on the RAID type.
- **Data Writing:** Data is written in small chunks from disk to disk and repeated until all data is completely written in a distributed fashion, happening in parallel.
- **Throughput:** If one disk has a throughput of 50 MB/s, the combined throughput achieved by all the disks will be the independent throughput multiplied by the number of disks in the RAID group.
- **Fault Tolerance:** RAID 0 is not designed for fault tolerance or redundancy, just for speed.

### RAID 1
- **Virtual Hard Disk:** Presented as a virtual hard disk to the server.
- **Data Mirroring:** The server writes a block to a virtual hard disk, and the RAID controller writes to two different hard disks that are mirror copies of each other. Three-way mirror copies are also possible.
- **Capacity:** Mirroring requires twice the capacity.
- **Design:** RAID 1 is designed for fault tolerance and fast read speeds, but write speeds are reduced due to the copying of data.

### RAID 10 (1+0)
- **Two-Stage Virtualization Hierarchy:** RAID 10 involves two stages of virtualization.
- **Virtual Storage:** RAID 10 has multiple virtual storage groups, each implementing RAID 0. These groups are then mirrored, effectively combining RAID 0 and RAID 1.
- **Data Striping:** Each group of disks experiences RAID 0, where data is striped across the disks.
- **Server View:** Despite having multiple virtual hard disks, the server sees only one.
- **Data Arrangement:** Data is first striped across the disks within each RAID 0 group and then mirrored between groups. For example, with eight physical disks, there are four RAID 0 arrays that are mirrored to create a single RAID 10 array.

### Notes on RAID 01 (0+1)
- **RAID 01 (0+1):** Involves striping data across multiple mirrored sets. It is less fault-tolerant than RAID 10 because a single disk failure in a mirrored set can make the entire array vulnerable.
- **Comparison:** RAID 10 is generally preferred due to better fault tolerance as it can survive multiple disk failures if the failures are in different mirrored sets.





