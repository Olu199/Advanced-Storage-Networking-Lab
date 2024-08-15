### 802.1Q VLAN Tagging

- **802.1Q** inserts a 32-bit tag between the Ethernet Type (ET) and the Source MAC address. This standard allows for a minimum frame size of 64 bytes and a maximum of 1518 bytes (including the VLAN tag). The purpose of 802.1Q is to enable multiple VLANs to operate over the same Layer 2 physical network by separating each VLAN into its own broadcast domain, ensuring they are isolated from each other.

- **802.1Q and Switch Ports:**
  - **Access Ports:** Typically assigned to a single VLAN ID, access ports handle untagged traffic. When frames enter an access port, the switch adds the corresponding VLAN ID, then uses ARP and broadcasting to determine the destination of the frame. The frame is tagged with the VLAN ID and will only travel within the path of that VLAN.
  - **Trunk Ports:** These are connections between two 802.1Q-capable devices. Trunk ports carry traffic from multiple VLANs simultaneously by tagging frames with the appropriate VLAN ID, allowing for the separation of broadcast domains across different network segments.

- **802.1Q VLAN Benefits:**
  - Creates separate, isolated Layer 2 network segments.
  - Establishes distinct broadcast domains, reducing broadcast traffic within the network.

### 802.1AD (QinQ)

- **802.1AD** (QinQ or Provider Bridging) extends VLAN tagging by adding a second VLAN tag, known as a service tag (S-tag), to prevent VLAN ID collisions. This allows Internet Service Providers (ISPs) or carriers to manage customer traffic using VLANs across their networks, even when customers are using their own VLANs. The nested VLANs (QinQ) provide an additional layer of isolation and flexibility, particularly useful in scenarios involving large-scale service providers.

### Differences Between 802.1Q and 802.1AD

- **802.1Q** focuses on VLAN tagging within a single Layer 2 domain, enabling the creation of isolated broadcast domains on the same physical network.
- **802.1AD (QinQ)** adds another layer of VLAN tagging, allowing for the encapsulation of customer VLANs within service provider VLANs, ensuring that multiple layers of VLANs can coexist without interference.
