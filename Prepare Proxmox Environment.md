
#### Network Configuration for Proxmox VE/Node 1:
| vmbr Interface | Description                                                                        |
| -------------- | ---------------------------------------------------------------------------------- |
| **vmbr0**      | Management port for Proxmox hypervisor, connected to the switch.                   |
| **vmbr1**      | Intercluster network for ONTAP nodes, isolated and utilizing VLAN tags.            |
| **vmbr2**      | Data and management network for ONTAP Cluster 7.                                   |
| **vmbr3**      | Data and management network for ONTAP Cluster 14.                                  |
| **vmbr4**      | Data and management network for ONTAP Cluster 14.2.                                |
| **vmbr5**      | First port on the network interface card, connected to the internal router (VyOS). |
| **vmbr6**      | Used for ESXi servers, connected to VyOS router.                                   |

#### Network Configuration for Proxmox VE/Node 2:
| vmbr Interface | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| **vmbr0**      | Management port for Proxmox hypervisor, connected to the switch.            |
| **vmbr1**      | Intercluster network for ONTAP nodes, isolated and utilizing VLAN tags.     |
| **vmbr2**      | Data and management network for ONTAP Cluster 7.                            |
| **vmbr3**      | Data and management network for ONTAP Cluster 14.                           |
| **vmbr4**      | Data and management network for ONTAP Cluster 14.2.                         |
| **vmbr5**      | First port on the network interface card, connected to the internal router (VyOS). |
| **vmbr6**      | Used for ESXi servers, connected to VyOS router.                            |

### Setup Guide:

#### Proxmox VE Installation:
1. Download and install Proxmox VE on both nodes.
2. Plug the inbuilt network port for both nodes into the switch.
3. Plug the first port of the network interface Ethernet card you purchased for both nodes into the switch using RJ45 cables.
4. Plug your switch into the router. In total, the switch should have 5 ports in use: 4 going to the PVE and PVE1 combined, and 1 going to your router.
5. Configure the network interfaces in Proxmox VE:
   - Assign vmbr0 for management (it is assigned by default; otherwise, assign it manually).

| Node 1 Interface | Comment                                    | Node 2 Interface | Comment                             |
| ---------------- | ------------------------------------------ | ---------------- | ----------------------------------- |
| **vmbr0**        | PVE intercluster                           | **vmbr0**        | PVE intercluster                    |
| **vmbr1**        | ONTAP intercluster switch port             | **vmbr1**        | Active Directory and Clients switch |
| **vmbr2**        | ONTAP Cluster 1 data and management switch | **vmbr2**        | n/a                                 |
| **vmbr3**        | ONTAP Cluster 2 data and management switch | **vmbr3**        | n/a                                 |
| **vmbr4**        | ONTAP Cluster 3 data and management switch | **vmbr4**        | n/a                                 |
| **vmbr5**        | VyOS interconnect bridge switch            | **vmbr5**        | VyOS interconnect bridge switch     |

Give vmbr5 the first port on the network interface card as a bridge port for both nodes. It should look like this:

![[vmbradapters1-6pve.png]]

This concludes the physical setup.

[[Proxmox Lab Setup and Configuration Guide|TBC]]