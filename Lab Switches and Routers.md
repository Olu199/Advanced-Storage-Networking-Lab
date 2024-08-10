
# Network Setup Documentation

**Note:**  
*It is easier to set up your Proxmox lab using the serial console. A wrong SSH network configuration might lock you out, but with the serial console, you can easily recover from mistakes and save energy while typing.*

In Proxmox, you can access the serial console of a VM that has a serial device by following these steps. **Remember to replace `115` with your specific VM ID or QEMU ID**:

```shell
root@pve:~# qm set 115 -serial0 socket
update VM 115: -serial0 socket
root@pve:~# qm start 115
root@pve:~# qm terminal 115
starting serial terminal on interface serial0 (press Ctrl+O to exit)
```
---

## Overview

This document provides detailed information about the configuration and setup of the `mgmt`, `storage`, `storage interconnect`, and `data` devices within the network for both Site 1 and Site 2. Each device has specific roles and responsibilities to ensure proper routing, NAT, and communication between various network segments.

### Device Roles
- **Management (`mgmt`) Device:** Handles overall network management, routing, and SSH access.
- **Storage (`storage`) Device:** Manages storage subnets and routes traffic to the storage network.
- **Storage Interconnect:** Acts as a cluster switch for creating inter-cluster networks, with only the interconnect address needed.
- **Data (`data`) Device:** Manages the data traffic network and routes traffic between storage and management networks.

---

## 1. Site 1 Configuration

### 1.1 `mgmt` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.1/24
set interfaces ethernet eth0 address 10.1.10.61/24
set interfaces ethernet eth1 address 192.168.1.1/24
```

#### **Routing Setup:**
```shell
configure
delete protocols static route 
commit
set protocols static route 0.0.0.0/0 next-hop 10.1.10.1 
set protocols static route 172.16.0.0/24 next-hop 192.168.0.2  
set protocols static route 172.16.1.0/24 next-hop 192.168.0.2  
set protocols static route 172.16.2.0/24 next-hop 192.168.0.2  
set protocols static route 172.18.1.0/24 next-hop 192.168.0.3  
set protocols static route 172.18.2.0/24 next-hop 192.168.0.4  
set protocols static route 192.168.2.0/24 next-hop 192.168.0.4 
commit
save
```

#### **NAT Configuration:**
```shell
set nat source rule 100 description 'NAT for internet access for 192.168.1.0/24'
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 192.168.1.0/24
set nat source rule 100 translation address masquerade
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.1
set service ssh listen-address 10.1.10.61
```

#### **System Settings:**
```shell
set system name-server 8.8.8.8
set system name-server 8.8.4.4
set system host-name mgmt
commit
save
```

### 1.2 `storage` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.2/24
set interfaces ethernet eth0 address 10.1.10.62/24
set interfaces ethernet eth1 address 172.16.0.1/24
set interfaces ethernet eth2 address 172.16.1.1/24
set interfaces ethernet eth3 address 172.16.2.1/24
```

#### **Routing Setup:**
```shell
set protocols static route 192.168.1.0/24 next-hop 192.168.0.1
set protocols static route 0.0.0.0/0 next-hop 192.168.0.1
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.2
set service ssh listen-address 10.1.10.62
```

#### **System Settings:**
```shell
set system host-name storage
commit
save
```

### 1.3 `storage interconnect` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 169.254.1.1/16
commit
save
```

> **Note:** The storage interconnect device serves as a cluster switch and requires only the interconnect address. No SSH or additional configuration is needed.

### 1.4 `data` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.3/24
set interfaces ethernet eth0 address 10.1.10.63/24
set interfaces ethernet eth1 address 172.18.1.1/24
```

#### **Routing Setup:**
```shell
set protocols static route 192.168.1.0/24 next-hop 192.168.0.1
set protocols static route 0.0.0.0/0 next-hop 192.168.0.1
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.3
set service ssh listen-address 10.1.10.63
```

#### **System Settings:**
```shell
set system host-name data
commit
save
```

---

## 2. Site 2 Configuration

### 2.1 `mgmt` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.4/24
set interfaces ethernet eth0 address 10.1.10.64/24
set interfaces ethernet eth1 address 192.168.2.1/24
```

#### **Routing Setup:**
```shell
configure
delete protocols static route 
commit
set protocols static route 0.0.0.0/0 next-hop 10.1.10.1
set protocols static route 172.16.6.0/24 next-hop 192.168.0.5   
set protocols static route 172.16.7.0/24 next-hop 192.168.0.5   
set protocols static route 172.16.8.0/24 next-hop 192.168.0.5   
set protocols static route 172.18.2.0/24 next-hop 192.168.0.6   
set protocols static route 172.18.1.0/24 next-hop 192.168.0.1   
set protocols static route 192.168.1.0/24 next-hop 192.168.0.1  
commit
save
```

#### **NAT Configuration:**
```shell
set nat source rule 100 description "NAT for internet access for 192.168.2.0/24"
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 192.168.2.0/24
set nat source rule 100 translation address masquerade
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.4
set service ssh listen-address 10.1.10.64
```

#### **System Settings:**
```shell
set system name-server 8.8.8.8
set system name-server 8.8.4.4
set system host-name mgmt2
commit
save
```

### 2.2 `storage` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.5/24
set interfaces ethernet eth0 address 10.1.10.65/24
set interfaces ethernet eth1 address 172.16.6.1/24
set interfaces ethernet eth2 address 172.16.7.1/24
set interfaces ethernet eth3 address 172.16.8.1/24
```

#### **Routing Setup:**
```shell
set protocols static route 192.168.2.0/24 next-hop 192.168.0.4
set protocols static route 0.0.0.0/0 next-hop 192.168.0.4
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.5
set service ssh listen-address 10.1.10.65
```

#### **System Settings:**
```shell
set system host-name storage2
commit
save
```

### 2.3 `storage interconnect` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 169.254.1.1/16
commit


save
```

> **Note:** The storage interconnect device serves as a cluster switch and requires only the interconnect address. No SSH or additional configuration is needed.

### 2.4 `data` Device

#### **Interface Configuration:**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.6/24
set interfaces ethernet eth0 address 10.1.10.66/24
set interfaces ethernet eth1 address 172.18.2.1/24
```

#### **Routing Setup:**
```shell
set protocols static route 192.168.2.0/24 next-hop 192.168.0.4
set protocols static route 0.0.0.0/0 next-hop 192.168.0.4
```

#### **SSH Access:**
```shell
set service ssh listen-address 192.168.0.6
set service ssh listen-address 10.1.10.66
```

#### **System Settings:**
```shell
set system host-name data2
commit
save
```

---

## 3. Additional Information

### 3.1 IP Addressing Scheme and Access

| **Network Range**    | **Devices**        | **IP Addresses**     | **Accessible Networks**                           | **Internet Access** |
|----------------------|--------------------|----------------------|---------------------------------------------------|---------------------|
| **192.168.0.0/24**   | mgmt (Site 1)      | 192.168.0.1 ✓        | 10.1.10.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 | ✓                   |
|                      | storage (Site 1)   | 192.168.0.2          | 10.1.10.0/24, 192.168.0.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 |                     |
|                      | data (Site 1)      | 192.168.0.3          | 10.1.10.0/24, 192.168.0.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 |                     |
| **192.168.0.0/24**   | mgmt (Site 2)      | 192.168.0.4 ✓        | 10.1.10.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 | ✓                   |
|                      | storage (Site 2)   | 192.168.0.5          | 10.1.10.0/24, 192.168.0.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 |                     |
|                      | data (Site 2)      | 192.168.0.6          | 10.1.10.0/24, 192.168.0.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 |                     |
| **10.1.10.0/24**     | mgmt (Site 1)      | 10.1.10.61 ✓         | 192.168.0.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 | ✓                   |
|                      | storage (Site 1)   | 10.1.10.62           | 192.168.0.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 |                     |
|                      | data (Site 1)      | 10.1.10.63           | 192.168.0.0/24, 192.168.1.0/24, 172.16.x.x/24, 172.18.1.0/24 |                     |
| **10.1.10.0/24**     | mgmt (Site 2)      | 10.1.10.64 ✓         | 192.168.0.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 | ✓                   |
|                      | storage (Site 2)   | 10.1.10.65           | 192.168.0.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 |                     |
|                      | data (Site 2)      | 10.1.10.66           | 192.168.0.0/24, 192.168.2.0/24, 172.16.x.x/24, 172.18.2.0/24 |                     |
| **192.168.1.0/24**   | Routed by mgmt (Site 1) | N/A                  | 10.1.10.0/24, 192.168.0.0/24, 172.16.x.x/24, 172.18.1.0/24 | ✓                   |
| **192.168.2.0/24**   | Routed by mgmt (Site 2) | N/A                  | 10.1.10.0/24, 192.168.0.0/24, 172.16.x.x/24, 172.18.2.0/24 | ✓                   |
| **172.16.x.x/24**    | Managed by storage (Site 1 & 2) | N/A                  | 192.168.0.0/24, 10.1.10.0/24, 172.18.x.x/24        |                     |
| **172.18.x.x/24**    | Managed by data (Site 1 & 2)    | N/A                  | 192.168.0.0/24, 10.1.10.0/24, 172.16.x.x/24        |                     |
| **169.254.0.0/16**   | Storage Interconnect (Site 1 & 2) | N/A                | 172.16.x.x/24                                      |                     |

### Legend:
- **✓**: Indicates that the IP or network has internet access.
- **172.16.x.x/24**: Represents the combined storage subnets (172.16.0.0/24, 172.16.1.0/24, 172.16.2.0/24 for Site 1; 172.16.6.0/24, 172.16.7.0/24, 172.16.8.0/24 for Site 2).

### 3.2 Routing and NAT

| Configuration Type   | Details                                                                 |
|----------------------|-------------------------------------------------------------------------|
| Default Routes       | All devices use their respective mgmt device as the gateway for reaching external networks. |
| Static Routes        | Specific subnets are routed through appropriate next-hops to ensure proper network segmentation and traffic flow. |
| NAT                  | `mgmt` devices perform NAT for the 192.168.x.x/24 subnets to provide internet access. |

### 3.3 Security Considerations

| Security Aspect      | Details                                                                 |
|----------------------|-------------------------------------------------------------------------|
| SSH Access           | Limited to specific IP addresses to minimize exposure.                  |
| NAT Rules            | Configured to provide internet access while protecting internal network structure. |
| Routing              | Static routes ensure only necessary traffic is routed between subnets, preventing unnecessary exposure. |

### 3.4 Future Enhancements

| Enhancement Area     | Suggestions                                                             |
|----------------------|-------------------------------------------------------------------------|
| Redundancy           | Implement additional NAT rules or secondary routes to increase redundancy. |
| Security             | Consider further restrictions on SSH access or additional firewall rules. |
| Monitoring           | Integrate network monitoring tools to keep track of network health and performance. |

### 3.5 Troubleshooting Tips

| Issue Area           | Troubleshooting Steps                                                   |
|----------------------|-------------------------------------------------------------------------|
| Connectivity Issues  | Verify IP addressing and subnet configurations.                         |
|                      | Check static routes for accuracy.                                       |
| NAT Problems         | Ensure NAT rules are correctly configured on the `mgmt` devices.        |
| SSH Access           | Confirm that SSH is listening on the correct interfaces and IP addresses. |

### 3.6 Backup and Restore Instructions

| Action               | Command                                                                 |
|----------------------|-------------------------------------------------------------------------|
| Backup Configuration | `save /config/config.boot`                                              |
| Restore Configuration| `load /config/config.boot`<br>`commit`<br>`save`                         |

### 3.7 Conclusion

| Section              | Summary                                                                 |
|----------------------|-------------------------------------------------------------------------|
| Documentation Summary| This documentation provides a comprehensive overview of the current network setup, focusing on the `mgmt`,

 `storage`, `storage interconnect`, and `data` devices for both Site 1 and Site 2. The setup is designed for efficiency, security, and ease of management. Future improvements can be made to enhance redundancy, security, and monitoring. |
