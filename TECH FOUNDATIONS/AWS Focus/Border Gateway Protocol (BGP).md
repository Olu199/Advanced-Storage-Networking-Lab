### Overview

Border Gateway Protocol (BGP) is a standardized exterior gateway protocol designed to exchange routing and reachability information among autonomous systems (ASes) on the internet. Unlike interior routing protocols like OSPF or EIGRP, BGP operates across different autonomous systems, making it the protocol of choice for routing traffic between distinct networks.

### Key Concepts

- **Autonomous System (AS):**
  - An AS is a collection of IP networks and routers under the control of a single organization that presents a common routing policy to the internet. Each AS is assigned a unique AS Number (ASN) by the Internet Assigned Numbers Authority (IANA).
  - ASN ranges:
    - Public: 0-64511
    - Private: 64512-65534
  - ASNs are typically 16-bit, but 32-bit ASNs are also in use.

- **Reliability & Configuration:**
  - BGP operates over TCP port 179, ensuring reliable delivery of its messages.
  - Peering between BGP routers is manually configured, making the protocol reliable but not automated. This means network administrators must establish and manage the BGP connections themselves.

- **Path-Vector Protocol:**
  - BGP is a path-vector protocol, meaning it selects the best path to a destination based on various attributes and exchanges this path information between peers.
  - It is not concerned with speed but focuses on ensuring that at least one reliable path, or multiple reliable paths, are available.

- **ASPATH (Autonomous System Path):**
  - ASPATH is a key attribute in BGP that records the ASes a route has traversed. This helps in preventing routing loops and influences route selection.

### BGP in Cloud Environments

- **Internal BGP (iBGP):**
  - iBGP is used for routing within a single AS. It helps in distributing routing information between routers inside the same AS.

- **External BGP (eBGP):**
  - eBGP is used for routing between different ASes. It is the primary mechanism for routing on the global internet.

### Example: BGP in Action

In the image provided, we see a simplified BGP topology across different regions in Australia, represented by different ASes:

- **Brisbane (ASN 200):**
  - Network: 10.16.0.0/16
  - Routes to: 10.17.0.0/16 via ASN 201
  - Routes to: 10.18.0.0/16 via ASN 202

- **Adelaide (ASN 201):**
  - Network: 10.17.0.0/16
  - Routes to: 10.16.0.0/16 via ASN 200
  - Routes to: 10.18.0.0/16 via ASN 202

- **Alice Springs (ASN 202):**
  - Network: 10.18.0.0/16
  - Routes to: 10.16.0.0/16 via ASN 200
  - Routes to: 10.17.0.0/16 via ASN 201

#### Network Routes Example:

| Destination  | Next Hop  | ASPATH |
| ------------ | --------- | ------ |
| 10.16.0.0/16 | 0.0.0.0   | i      |
| 10.17.0.0/16 | 10.17.0.1 | 201,i  |
| 10.18.0.0/16 | 10.18.0.1 | 202,i  |

In this setup:
- **i** indicates that the network was learned from a locally connected network.
- **ASPATH** shows the sequence of ASes that the route has passed through.

### Comparison to ALUA

- BGP can be compared to ALUA (Asymmetric Logical Unit Access) in storage networking, which defines the optimal path for data access. Similarly, BGP determines the best path for data routing across networks, though BGP is focused on reliability and reachability rather than speed.

### AS Path Prepending

AS Path Prepending is a technique used to influence routing decisions by artificially increasing the AS path length, making a specific path less attractive to BGP peers. This can be particularly useful when you want to avoid using a shorter but less desirable route and instead route traffic through a longer but more reliable or higher-bandwidth path.

**Example Scenario:**

Between Alice Springs and Brisbane, the shortest path is through a 5 Mbps satellite link. However, there is a more desirable path via a 1 Gbps fiber connection, even though it involves two hops through Adelaide. By configuring AS path prepending, you can make the satellite path appear less favorable, encouraging the use of the longer but higher-bandwidth fiber connection between Alice Springs and Brisbane.

This method ensures that critical traffic uses the optimal path in terms of performance and reliability rather than simply the shortest path.
