
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
## **IP Address Table**

| **Device (Site)**        | **Interfaces** | **IP Addresses**                    | **Subnet Mask**    |
|--------------------------|----------------|-------------------------------------|--------------------|
| **VyOS Router (A)**       | eth0           | 192.168.0.1, 10.1.10.61             | 255.255.255.0      |
|                          | eth1           | 198.18.0.1                          | 255.255.255.128    |
| **VyOS Router (B)**       | eth0           | 192.168.0.4, 10.1.10.64             | 255.255.255.0      |
|                          | eth1           | 198.18.1.1                          | 255.255.255.128    |
| **Storage Device (A)**    | eth0           | 192.168.0.2, 10.1.10.62             | 255.255.255.0      |
|                          | eth1           | 172.16.0.1                          | 255.255.255.0      |
|                          | eth2           | 172.16.1.1                          | 255.255.255.0      |
|                          | eth3           | 172.16.2.1                          | 255.255.255.0      |
| **Storage Device (B)**    | eth0           | 192.168.0.5, 10.1.10.65             | 255.255.255.0      |
|                          | eth1           | 172.16.6.1                          | 255.255.255.0      |
|                          | eth2           | 172.16.7.1                          | 255.255.255.0      |
|                          | eth3           | 172.16.8.1                          | 255.255.255.0      |
| **Data Device (A)**       | eth0           | 192.168.0.3, 10.1.10.63             | 255.255.255.0      |
|                          | eth1           | 172.18.1.1                          | 255.255.255.0      |
| **Data Device (B)**       | eth0           | 192.168.0.6, 10.1.10.66             | 255.255.255.0      |
|                          | eth1           | 172.18.2.1                          | 255.255.255.0      |


### **Site 1**

#### **VyOS Router Configuration for Management Device**

1. **Clear Previous Configuration**
```bash
configure
delete interfaces ethernet
delete protocols static route
delete nat source rule
delete service ssh
commit
save
```

2. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.1/24
set interfaces ethernet eth0 address 10.1.10.61/24
set interfaces ethernet eth1 address 198.18.0.1/25
commit
save
```

3. **Routing Setup**
```shell
configure
delete protocols static route 
commit
set protocols static route 0.0.0.0/0 next-hop 10.1.10.1 
set protocols static route 172.16.0.0/24 next-hop 192.168.0.2  
set protocols static route 172.16.1.0/24 next-hop 192.168.0.2  
set protocols static route 172.16.2.0/24 next-hop 192.168.0.2  
set protocols static route 172.18.1.0/24 next-hop 192.168.0.3
set protocols static route 198.18.1.0/25 next-hop 192.168.0.4
set protocols static route 172.16.6.0/24 next-hop 192.168.0.5
set protocols static route 172.16.7.0/24 next-hop 192.168.0.5
set protocols static route 172.16.8.0/24 next-hop 192.168.0.5
set protocols static route 172.18.2.0/24 next-hop 192.168.0.6
commit
save
```

4. **Internet Access**
```shell
configure
set nat source rule 100 description 'NAT for internet access for 198.18.0.0/25'
set nat source rule 100 outbound-interface name eth0
set nat source rule 100 source address 198.18.0.0/25
set nat source rule 100 translation address masquerade
commit
save
```

5. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.1
set service ssh listen-address 10.1.10.61
commit
save
```

6. **System Settings**
```shell
configure
set system name-server 8.8.8.8
set system name-server 8.8.4.4
set system host-name mgmt
commit
save
```

#### **Storage Device Configuration**
1. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.2/24
set interfaces ethernet eth0 address 10.1.10.62/24
set interfaces ethernet eth1 address 172.16.0.1/24
set interfaces ethernet eth2 address 172.16.1.1/24
set interfaces ethernet eth3 address 172.16.2.1/24
commit
save
```

2. **Routing Setup**
```shell
configure
delete protocols static route 
commit
set protocols static route 198.18.0.0/25 next-hop 192.168.0.1
set protocols static route 0.0.0.0/0 next-hop 192.168.0.1
commit
save
```

3. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.2
set service ssh listen-address 10.1.10.62
commit
save
```

4. **System Settings**
```shell
configure
set system host-name storage
commit
save
```

#### **Data Device Configuration**
1. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.3/24
set interfaces ethernet eth0 address 10.1.10.63/24
set interfaces ethernet eth1 address 172.18.1.1/24
commit
save
```

2. **Routing Setup**
```shell
configure
set protocols static route 198.18.0.0/25 next-hop 192.168.0.1
set protocols static route 0.0.0.0/0 next-hop 192.168.0.1
commit
save
```

3. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.3
set service ssh listen-address 10.1.10.63
commit
save
```

4. **System Settings**
```shell
configure
set system host-name data
commit
save
```

---

### **Site 2**

#### **VyOS Router Configuration for Management Device**
**Network:** 198.18.1.0/25

1. **Clear Previous Configuration**
```bash
configure
delete interfaces ethernet
delete protocols static route
delete nat source rule
delete service ssh
commit
save
```

2. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.4/24
set interfaces ethernet eth0 address 10.1.10.64/24
set interfaces ethernet eth1 address 198.18.1.1/25
commit
save
```

3. **Routing Setup**
```shell
configure
delete protocols static route 
commit
set protocols static route 0.0.0.0/0 next-hop 10.1.10.1 
set protocols static route 172.16.6.0/24 next-hop 192.168.0.5  
set protocols static route 172.16.7.0/24 next-hop 192.168.0.5  
set protocols static route 172.16.8.0/24 next-hop 192.168.0.5  
set protocols static route 172.18.2.0/24 next-hop 192.168.0.6
set protocols static route 198.18.0.0/25 next-hop 192.168.0.1
set protocols static route 172.16.0.0/24 next-hop 192.168.0.2
set protocols static route 172.16.1.0/24 next-hop 192.168.0.2
set protocols static route 172.16.2.0/24 next-hop 192.168.0.2
set protocols static route 172.18.1.0/24 next-hop 192.168.0.3
commit
save
```

4. **Internet Access**
```shell
configure
set nat source rule 100 description 'NAT for internet access for 198.18.1.0/25'
set nat source rule 100 outbound-interface name eth0
set nat source rule 100 source address 198.18.1.0/25
set nat source rule 100 translation address masquerade
commit
save
```

5. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.4
set service ssh listen-address 10.1.10.64
commit
save
```

6. **System Settings**
```shell
configure
set system name-server 8.8.8.8
set system name-server 8.8.4.4
set system host-name mgmt
commit
save
```

#### **Storage Device Configuration**
1. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.5/24
set interfaces ethernet eth0 address 10.1.10.65/24
set interfaces ethernet eth1 address 172.16.6.1/24
set interfaces ethernet eth2 address 172.16.7.1/24
set interfaces ethernet eth3 address 172.16.8.1/24
commit
save
```

2. **Routing Setup**
```shell
configure
set protocols static route 198.18.2.0/25 next-hop 192.168.0.4
set protocols static route 0.0.0.0/0 next-hop 192.168.0.4
commit
save
```

3. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.5
set service ssh listen-address 10.1.10.65
commit
save
```

4. **System Settings**
```shell
configure
set system host-name storage
commit
save
```

#### **Data Device Configuration**
1. **Setup Interface**
```shell
configure
set interfaces ethernet eth0 address 192.168.0.6/24
set interfaces ethernet eth0 address 10.1.10.66/24
set interfaces ethernet eth1 address 172.18.2.1/24
commit
save
```

2. **Routing Setup**
```shell
configure
set protocols static route 198.18.2.0/25 next-hop 192.168.0.4
set protocols static route 0.0.0.0/0 next-hop 192.168.0.4
commit
save
```

3. **SSH Access**
```shell
configure
set service ssh listen-address 192.168.0.6
set service ssh listen-address 10.1.10.66
commit
save
```

4. **System Settings**
```shell
configure
set system host-name data
commit
save
```

