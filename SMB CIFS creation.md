
### Creating CIFS SVM with ONTAP

This guide outlines the steps to create a CIFS SVM (`cifsa`) with the appropriate network interfaces, DNS settings, volume, and CIFS share, ensuring redundancy.

#### 1. Create SVM

```bash
vserver create -vserver cifsa -rootvolume cifs_root -rootvolume-security-style ntfs
```

#### 2. Configure Network Interfaces

- **For `cifsa` (Node 1):**

```bash
network interface create -vserver cifsa -lif cifsa_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.18.1.2 -netmask-length 24
network interface create -vserver cifsa -lif cifsa_lif2 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0h -address 172.18.1.3 -netmask-length 24
```

- **For `cifsa` (Node 2):**

```bash
network interface create -vserver cifsa -lif cifsa_lif3 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0g -address 172.18.1.4 -netmask-length 24
network interface create -vserver cifsa -lif cifsa_lif4 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.18.1.5 -netmask-length 24
```

These configurations ensure that:
- `cifsa_lif1` and `cifsa_lif2` on Node 1 are connected to switches `a` and `b` using ports `e` and `f`.
- `cifsa_lif3` and `cifsa_lif4` on Node 2 are also connected to switches `a` and `b` using ports `e` and `f`.
  
This setup provides redundancy across both nodes.

#### 3. Configure Network Routing

- **For `cifsa`:**

```bash
net route create -vserver cifsa -destination 0.0.0.0/0 -gateway 198.168.0.1
```

#### 4. Set Up DNS

- **For `cifsa`:**

```bash
vserver services dns create -vserver cifsa -domains wina.com -name-servers 198.168.0.2
```

#### 5. Create CIFS Server

- **For `cifsa`:**

```bash
vserver cifs create -vserver cifsa -cifs-server cifsa -domain wina.com
```

#### 6. Create and Configure Volume

- **For `cifsa`:**

```bash
volume create -vserver cifsa -aggregate aggr1_ontap7_02_fcal -volume cifs_vola -size 50MB -junction-path /cifs_vola
```

#### 7. Create CIFS Share

- **For `cifsa`:**

```bash
vserver cifs share create -vserver cifsa -share-name cifs_vola -path /cifs_vola
```

#### 8. Configure Share Access Control

- **Remove Default Access:**

```bash
vserver cifs share access-control delete -share cifs_vola -user-or-group Everyone
```

- **Add Specific Access:**

```bash
vserver cifs share access-control create -share cifs_vola -user-or-group "Windows Users A" -user-group-type windows -permission Full_Control -vserver cifsa
```
