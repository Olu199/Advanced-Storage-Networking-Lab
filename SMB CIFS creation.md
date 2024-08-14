### Creating CIFS SVM with ONTAP

This guide outlines the steps to create a CIFS SVM (`cifsa`) with four LIFs across two nodes, ensuring redundancy by connecting to two switches.

#### 1. Create SVM

```bash
vserver create -vserver cifsa -rootvolume cifs_root -rootvolume-security-style ntfs
```

#### 2. Configure Network Interfaces

- **For `cifsa` on Node 1:**

```bash
network interface create -vserver cifsa -lif cifsa_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0e -address 172.18.1.2 -netmask-length 24
network interface create -vserver cifsa -lif cifsa_lif2 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0f -address 172.18.1.3 -netmask-length 24
```

- **For `cifsa` on Node 2:**

```bash
network interface create -vserver cifsa -lif cifsa_lif3 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0e -address 172.18.1.4 -netmask-length 24
network interface create -vserver cifsa -lif cifsa_lif4 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0f -address 172.18.1.5 -netmask-length 24
```

#### 3. Configure Network Routing

- **For `cifsa`:**

```bash
net route create -vserver cifsa -destination 0.0.0.0/0 -gateway 172.18.1.1
```

#### 4. Set Up DNS

- **For `cifsa`:**

```bash
vserver services dns create -vserver cifsa -domains wina.com -name-servers 172.18.1.10
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
