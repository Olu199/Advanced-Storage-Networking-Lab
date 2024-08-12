
### AFF (All-Flash FAS)
| NetApp AFF Models | Example Devices | Dell EMC | HP Enterprise | Pure Storage | VMware ESXi Equivalent or Related Feature |
|---|---|---|---|---|---|
| **AFF A800** | AFF A800, AFF A700 | PowerMax All Flash, Unity XT All Flash | HPE Primera, 3PAR All-Flash | FlashArray //X | vSAN All-Flash Configuration |
| **AFF C190** | AFF C190, AFF A220 | PowerMax All Flash, Unity XT All Flash | HPE Primera, Nimble All-Flash | FlashArray //C | vSAN All-Flash Configuration |
| **SnapMirror** | | SRDF / RecoverPoint | Remote Copy (RC) | ActiveCluster | vSphere Replication |
| **SnapVault** | | Data Domain / Avamar | StoreOnce | ActiveCluster | vSphere Data Protection / vSphere Replication |
| **MetroCluster** | | VPLEX Metro | Peer Persistence | ActiveCluster | vSAN Stretched Cluster |
| **Snapshot** | | Snapshots | Virtual Copy | SafeMode Snapshots | VMware Snapshots |
| **SVM** | | NAS Server (Unity) | File Persona | N/A (Supports multi-tenancy) | Virtual Machines with dedicated storage (vSAN or VMFS volumes) |
| **Volume** | | LUNs / Volumes | Virtual Volumes (VVOLs) | Volumes | Datastores (VMFS, NFS, or vSAN) |
| **ONTAP** | | PowerMax OS / Unity OE | HPE 3PAR OS / NimbleOS | Purity Operating Environment | N/A (Storage operating system, managed differently in VMware) |
| **SyncMirror** | | MirrorView / SRDF | Remote Copy (RC) | ActiveCluster | vSphere Replication with Synchronous Replication |
| **Thick Provisioning** | | Thick LUNs | Thick Volumes | Thick Volumes | Thick Provisioned Disk |
| **Thin Provisioning** | | Thin LUNs | Thin Volumes | Thin Volumes | Thin Provisioned Disk |
| **Space Reservation** | | Reserved Capacity (Unity) | Reserved Space (3PAR) | Reserved Capacity | VMFS/NFS Reserved Space |
| **Quality of Service (QoS)** | | FAST VP / FAST Cache | Adaptive QoS | QoS | Storage I/O Control (SIOC) |
| **Data Deduplication** | | Deduplication (Unity/PowerMax) | Deduplication | Deduplication | Deduplication (vSAN, VMFS/NFS via plugins) |
| **Compression** | | Compression (Unity/PowerMax) | Compression | Compression | vSAN Compression / Deduplication |
| **Compaction** | | Data Reduction (Unity) | Data Compaction | Data Reduction | vSAN Space Efficiency |
| **High Availability** | | PowerMax SRDF/HA | HPE 3PAR Persistent Ports | ActiveCluster | vSphere HA / FT |
| **Cluster** | | VPLEX Cluster | HPE Cluster Extension | FlashStack (with Cisco UCS) | VMware Cluster |
| **Node** | | PowerMax Node | HPE Node | FlashArray Controller Node | ESXi Host Node |
| **Disk Shelves** | | VMAX DAEs | HPE Drive Enclosures | N/A (Managed within external storage arrays) | N/A (Managed within external storage arrays) |
| **File Storage** | | Isilon / Unity NAS | HPE StoreEasy | FlashBlade | vSAN File Services / NFS/SMB Datastores |
| **Block Storage** | | PowerMax / VMAX / Unity Block | 3PAR / Primera / Nimble | FlashArray | VMFS Datastores / RDM |
| **Object Storage** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | FlashBlade / Pure ObjectEngine | vSAN Object Store (via plugins) |
| **Storage Grid** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | Pure Storage FlashBlade | N/A (Typically managed outside of VMware ESXi) |
| **S3 Compatible Storage** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | FlashBlade / Pure ObjectEngine | vCloud Director Object Storage Extension |

### FAS (Fabric-Attached Storage)
| NetApp FAS Models | Example Devices | Dell EMC | HP Enterprise | Pure Storage | VMware ESXi Equivalent or Related Feature |
|---|---|---|---|---|---|
| **FAS9000** | FAS9000, FAS8200 | Unity Hybrid, VMAX Hybrid | HPE 3PAR Hybrid, Nimble | FlashArray //C (for hybrid use cases) | VMFS Datastores backed by hybrid storage arrays |
| **FAS2750** | FAS2750, FAS2720 | Unity Hybrid, VMAX Hybrid | HPE Nimble Hybrid, 3PAR Hybrid | FlashArray //C | VMFS Datastores backed by hybrid storage arrays |
| **SnapMirror** | | SRDF / RecoverPoint | Remote Copy (RC) | ActiveCluster | vSphere Replication |
| **SnapVault** | | Data Domain / Avamar | StoreOnce | ActiveCluster | vSphere Data Protection / vSphere Replication |
| **MetroCluster** | | VPLEX Metro | Peer Persistence | ActiveCluster | vSAN Stretched Cluster |
| **Snapshot** | | Snapshots | Virtual Copy | SafeMode Snapshots | VMware Snapshots |
| **SVM** | | NAS Server (Unity) | File Persona | N/A (Supports multi-tenancy) | Virtual Machines with dedicated storage (vSAN or VMFS volumes) |
| **Volume** | | LUNs / Volumes | Virtual Volumes (VVOLs) | Volumes | Datastores (VMFS, NFS, or vSAN) |
| **ONTAP** | | PowerMax OS / Unity OE | HPE 3PAR OS / NimbleOS | Purity Operating Environment | N/A (Storage operating system, managed differently in VMware) |
| **SyncMirror** | | MirrorView / SRDF | Remote Copy (RC) | ActiveCluster | vSphere Replication with Synchronous Replication |
| **Thick Provisioning** | | Thick LUNs | Thick Volumes | Thick Volumes | Thick Provisioned Disk |
| **Thin Provisioning** | | Thin LUNs | Thin Volumes | Thin Volumes | Thin Provisioned Disk |
| **Space Reservation** | | Reserved Capacity (Unity) | Reserved Space (3PAR) | Reserved Capacity | VMFS/NFS Reserved Space |
| **Quality of Service (QoS)** | | FAST VP / FAST Cache | Adaptive QoS | QoS | Storage I/O Control (SIOC) |
| **Data Deduplication** | | Deduplication (Unity/PowerMax) | Deduplication | Deduplication | Deduplication (vSAN, VMFS/NFS via plugins) |
| **Compression** | | Compression (Unity/PowerMax) | Compression | Compression | vSAN Compression / Deduplication |
| **Compaction** | | Data Reduction (Unity) | Data Compaction | Data Reduction | vSAN Space Efficiency |
| **High Availability** | | PowerMax SRDF/HA | HPE 3PAR Persistent Ports | ActiveCluster | vSphere HA / FT |
| **Cluster** | | VPLEX Cluster | HPE Cluster Extension | FlashStack (with Cisco UCS) | VMware Cluster |
| **Node** | | PowerMax Node | HPE Node | FlashArray Controller Node | ESXi Host Node |
| **Disk Shelves** | | VMAX DAEs | HPE Drive Enclosures | N/A (Managed within external storage arrays) | N/A (Managed within external storage arrays) |
| **File Storage** | | Isilon / Unity NAS | HPE StoreEasy | FlashBlade | vSAN File Services / NFS/SMB Datastores |
| **Block Storage** | | PowerMax / VMAX / Unity Block | 3PAR / Primera / Nimble | FlashArray | VMFS Datastores / RDM |
| **Object Storage** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | FlashBlade / Pure ObjectEngine | vSAN Object Store (via plugins) |
| **Storage Grid** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | Pure Storage FlashBlade | N/A (Typically managed outside of VMware ESXi) |
| **S3 Compatible Storage** | | ECS (Elastic Cloud Storage) | HPE Apollo / Scality | FlashBlade / Pure ObjectEngine | vCloud Director Object Storage Extension |
