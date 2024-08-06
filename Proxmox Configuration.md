### Network Topology Overview

- **Node A (Router 1) where storage is located:**
    
    - Subnets: `172.16.0.0/19`, `172.16.32.0/19`, `172.16.64.0/19`
    - Internal network interface: `eth3` (connected to `192.168.0.0/24`)
    - Default route through `10.1.10.1`
- **Node B (Router 2) where computers are located:**
    
    - Subnets: `172.16.96.0/19`, `172.16.128.0/19`, `172.16.160.0/19`, `172.16.192.0/19`
    - Internal network interface: `eth1` (connected to `192.168.0.0/24`)
    - Default route through `192.168.0.1`
- **Common Networks:**
    
    - `192.168.0.0/24` for inter-router communication
    - External internet access through a shared gateway

### Step-by-Step Configuration

#### 1. VyOS Router Configuration

**Node A Router (Router 1):**

1. **Set Up Interfaces:**
    
    bash
    
    Copy code
    
    `configure set interfaces ethernet eth0 address '172.16.0.1/19' set interfaces ethernet eth1 address '172.16.32.1/19' set interfaces ethernet eth2 address '172.16.64.1/19' set interfaces ethernet eth3 address '192.168.0.1/24' set interfaces ethernet eth3 address '10.1.10.60/24' commit save exit`
    
2. **Configure Static Routes:**
    
    bash
    
    Copy code
    
    `configure set protocols static route 0.0.0.0/0 next-hop '10.1.10.1' set protocols static route 172.16.96.0/19 next-hop '192.168.0.2' set protocols static route 172.16.128.0/19 next-hop '192.168.0.2' set protocols static route 172.16.160.0/19 next-hop '192.168.0.2' set protocols static route 172.16.192.0/19 next-hop '192.168.0.2' commit save exit`
    
3. **Configure NAT:**
    
    bash
    
    Copy code
    
    `configure set nat source rule 10 description 'NAT for internal networks' set nat source rule 10 outbound-interface 'eth3' set nat source rule 10 source address '172.16.0.0/16' set nat source rule 10 translation address 'masquerade' commit save exit`
    

**Node B Router (Router 2):**

1. **Set Up Interfaces:**
    
    bash
    
    Copy code
    
    `configure set interfaces ethernet eth0 address '172.16.96.1/19' set interfaces ethernet eth0 address '172.16.128.1/19' set interfaces ethernet eth0 address '172.16.160.1/19' set interfaces ethernet eth0 address '172.16.192.1/19' set interfaces ethernet eth1 address '192.168.0.2/24' set interfaces ethernet eth1 address '10.1.10.61/24' commit save exit`
    
2. **Configure Static Routes:**
    
    bash
    
    Copy code
    
    `configure set protocols static route 0.0.0.0/0 next-hop '192.168.0.1' set protocols static route 172.16.0.0/19 next-hop '192.168.0.1' set protocols static route 172.16.32.0/19 next-hop '192.168.0.1' set protocols static route 172.16.64.0/19 next-hop '192.168.0.1' commit save exit`
    
3. **Configure NAT:**
    
    bash
    
    Copy code
    
    `configure set nat source rule 20 description 'NAT for internal networks' set nat source rule 20 outbound-interface 'eth1' set nat source rule 20 source address '172.16.96.0/19' set nat source rule 20 source address '172.16.128.0/19' set nat source rule 20 source address '172.16.160.0/19' set nat source rule 20 source address '172.16.192.0/19' set nat source rule 20 translation address 'masquerade' commit save exit`
    

#### 2. Server Configuration

**Windows Servers (WinA, WinB, NTP/DHCP/IPAM Server):**

1. **Set Static IP and DNS:**
    
    - Open **Control Panel** > **Network and Sharing Center**.
    - Click on **Change adapter settings**.
    - Right-click your network connection and select **Properties**.
    - Select **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
    - Set the IP address, subnet mask, default gateway, and DNS server.
    
    Example for NTP/DHCP/IPAM Server:
    
    plaintext
    
    Copy code
    
    `IP Address: 172.16.192.10 Subnet Mask: 255.255.224.0 Default Gateway: 172.16.192.1 Preferred DNS Server: 172.16.96.10 (WinA)`
    
2. **Add Persistent Routes (if necessary):**
    
    - Open Command Prompt as Administrator.
    - Add routes to ensure proper communication.
    
    cmd
    
    Copy code
    
    `route add 0.0.0.0 mask 0.0.0.0 172.16.192.1 -p`
    
3. **Flush DNS Cache and Register DNS:**
    
    cmd
    
    Copy code
    
    `ipconfig /flushdns ipconfig /registerdns`

## Introduction

This guide provides detailed instructions for setting up a Proxmox lab environment. It covers everything from initial setup to advanced configurations, making it suitable for students at any stage of their career.

## Prerequisites

Before starting, ensure you have the following:

- Proxmox VE installed and running
- Basic knowledge of Linux and network configurations
- Access to the required software and tools

## Step 1: Prepare Proxmox Environment

### Importing ONTAP Simulator

1. Extract the file:
    ```bash
    tar -xvf <file_name>
    ```

2. Import the OVF to Proxmox:
    ```bash
    qm importovf 122 "9.1.0.0.ovf" local-lvm
    ```

3. Import the disks:
    ```bash
    qm disk import 101 vsim-NetAppDOT-simulate-disk1.vmdk local-lvm -format qcow2
    qm disk import 112 9.1.0.0-disk22.vmdk local-lvm -format qcow2
    ```

4. Set the disk:
    ```bash
    qm set 122 --scsi21 local-lvm:vm-122-disk-21
    ```

### Useful Links
- [GitHub - MRCzap/ontapsimulator](https://github.com/MRCzap/ontapsimulator)
- [Disable root volume snapshots in ONTAP 9](https://kb.netapp.com/on-prem/ontap/Ontap_OS/OS-KBs/How_to_disable_root_volume_snapshots_in_ONTAP_9)

## Step 2: Network Route Configuration

### Adding a Persistent Route to Windows Servers
```bash
route -p add 192.168.1.0 mask 255.255.255.0 10.0.0.1
route print
```

## Step 3: VyOS Configuration

### Show Static Routes
```bash
show ip route static
```

### Set Static Route
```bash
set protocol static route <targeted_network> next-hop <target_network_address>
```

### Delete a Route
```bash
route delete <Network_A>
netsh interface ipv4 add route <Network_A>/<Prefix_Length> <Interface> <Gateway_IP> store=persistent
```

## Step 4: Configure Proxmox Nodes and Devices

### Node and Device Setup

#### ONTAP Cluster Network Details

**Node 1 ONTAP Configuration:**
```bash
ontap14::> net int show
   (network interface show)

Vserver       Logical      Status     Network           Current      Is
              Interface    Admin/Oper Address/Mask      Node         Port     Home
----------  -----------  ----------  ---------------  ---------  ---------  -----
Cluster
 ontap14-01_clus1  up/up  169.254.127.153/16  ontap14-01  e0a  true
 ontap14-01_clus2  up/up  169.254.104.88/16   ontap14-01  e0b  true
 ontap14-02_clus1  up/up  169.254.52.28/16    ontap14-02  e0a  true
 ontap14-02_clus2  up/up  169.254.84.170/16   ontap14-02  e0b  true

ontap14
 cluster_mgmt      up/up  172.16.32.3/19     ontap14-01  e0d  true
 ontap14-01_mgmt1  up/up  172.16.32.2/19     ontap14-01  e0c  true

6 entries were displayed.
```

**Node 2 ONTAP Configuration:**
```bash
ontap142::> net int show
   (network interface show)

Vserver       Logical      Status     Network           Current      Is
              Interface    Admin/Oper Address/Mask      Node         Port     Home
----------  -----------  ----------  ---------------  ---------  ---------  -----
Cluster
 ontap142-01_clus1  up/up  169.254.247.95/16  ontap142-01  e0a  true
 ontap142-01_clus2  up/up  169.254.178.138/16 ontap142-01  e0b  true
 ontap142-02_clus1  up/up  169.254.199.98/16  ontap142-02  e0a  true
 ontap142-02_clus2  up/up  169.254.184.142/16 ontap142-02  e0b  true

ontap142
 cluster_mgmt      up/up  172.16.64.3/19     ontap142-01  e0d  true
 ontap142-01_mgmt1  up/up  172.16.64.2/19     ontap142-01  e0c  true

6 entries were displayed.
```

**Node 3 ONTAP Configuration:**
```bash
ontap7::> net int show
   (network interface show)

Vserver       Logical      Status     Network           Current      Is
              Interface    Admin/Oper Address/Mask      Node         Port     Home
----------  -----------  ----------  ---------------  ---------  ---------  -----
Cluster
 ontap7-01_clus1  up/up  169.254.128.167/16  ontap7-01  e0a  true
 ontap7-01_clus2  up/up  169.254.188.113/16  ontap7-01  e0b  true
 ontap7-02_clus1  up/up  169.254.22.60/16    ontap7-02  e0a  true
 ontap7-02_clus2  up/up  169.254.228.107/16  ontap7-02  e0b  true

ontap7
 cluster_mgmt        up/up  172.16.0.3/19     ontap7-01  e0c  true
 ontap7-01_mgmt      up/up  172.16.0.2/19     ontap7-01  e0c  true
 ontap7-02_mgmt_auto up/up  169.254.103.14/16 ontap7-02  e0c  true

7 entries were displayed.
```

### VyOS Configuration for Proxmox Nodes

**VyOS as Inter-Cluster Switch (Node 1):**
```bash
Interface    IP Address          MAC                VRF       MTU   S/L  Description
---------    ----------          ---                ---       ---   ---  -----------
eth0         169.254.0.1/16      bc:24:11:d3:fd:3b  default   1500  u/u
lo           127.0.0.1/8         00:00:00:00:00:00  default   65536 u/u
             ::1/128
```

**

VyOS as Router (Node 1):**
```bash
Interface    IP Address            MAC                VRF       MTU    S/L  Description
---------    ----------            ---                ---       ---    ---  -----------
eth0         172.16.0.1/19         bc:24:11:cd:0c:0c  default   1500   u/u
eth1         172.16.32.1/19        bc:24:11:c1:ac:30  default   1500   u/u
eth2         172.16.64.1/19        bc:24:11:2d:5e:b5  default   1500   u/u
eth3         192.168.0.1/24        bc:24:11:9f:87:64  default   1500   u/u
             10.1.10.60/24
lo           127.0.0.1/8           00:00:00:00:00:00  default   65536  u/u
             ::1/128

S>* 172.16.96.0/19 [1/0] via 192.168.0.2, eth3, weight 1, 02w3d10h
S>* 172.16.128.0/19 [1/0] via 192.168.0.2, eth3, weight 1, 02w3d10h
S>* 172.16.160.0/19 [1/0] via 192.168.0.2, eth3, weight 1, 02w3d10h
```

**VyOS as Router (Node 2):**
```bash
vyos@vyos:~$ show ip route static

S>* 172.0.0.0/19 [1/0] via 192.168.0.1, eth1, weight 1, 02w3d17h
S>* 172.16.0.0/19 [1/0] via 192.168.0.1, eth1, weight 1, 02w3d17h
S>* 172.16.32.0/19 [1/0] via 192.168.0.1, eth1, weight 1, 02w3d17h
S>* 172.16.64.0/19 [1/0] via 192.168.0.1, eth1, weight 1, 02w3d17h

vyos@vyos:~$ show interfaces
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface    IP Address           MAC                VRF       MTU    S/L  Description
---------    ----------           ---                ---       ---    ---  -----------
eth0         172.16.96.1/19       bc:24:11:2f:d4:7a  default   1500   u/u
             172.16.128.1/19
             172.16.160.1/19
eth1         192.168.0.2/24       bc:24:11:7d:26:7a  default   1500   u/u
lo           127.0.0.1/8          00:00:00:00:00:00  default   65536  u/u
             ::1/128
```

## Step 5: Persistent Route Configuration

**Location B Persistent Routes:**
```bash
Persistent Routes:
Network Address       Netmask           Gateway Address    Metric
172.16.64.0           255.255.224.0     192.168.0.1        1
```

**Location A Persistent Routes:**
```bash
Persistent Routes:
Network Address       Netmask           Gateway Address    Metric
172.16.0.0            255.255.224.0     192.168.0.1        1
172.16.32.0           255.255.224.0     192.168.0.1        1
```

## Step 6: Disk Management

**Disk Management Commands:**
```bash
lsblk
sudo fdisk -l /dev/sda*
mount | grep sdb
sudo mkfs.ext4 /dev/sda*
```

**Useful Links for Proxmox Storage Configuration:**
- [Proxmox Documentation on Storage Types](https://10.1.10.50:8006/pve-docs/chapter-pvesm.html#_storage_types)
- [Proxmox Wiki on Logical Volume Manager (LVM)](https://pve.proxmox.com/wiki/Logical_Volume_Manager_(LVM)#_creating_a_volume_group)

## Step 7: Setting Up Internet Service for Client PC

```bash
set protocols static route 0.0.0.0/0 next-hop <internet_gateway_ip>
set nat source rule 10 outbound-interface eth3
set nat source rule 10 source address 172.16.0.0/16
set nat source rule 10 translation address masquerade
```

## Step 8: Subnet Information

| Network ID   | First IP     | Last IP        | Broadcast Address |
| ------------ | ------------ | -------------- | ----------------- |
| 172.16.0.0   | 172.16.0.1   | 172.16.31.254  | 172.16.31.255     |
| 172.16.32.0  | 172.16.32.1  | 172.16.63.254  | 172.16.63.255     |
| 172.16.64.0  | 172.16.64.1  | 172.16.95.254  | 172.16.95.255     |
| 172.16.96.0  | 172.16.96.1  | 172.16.127.254 | 172.16.127.255    |
| 172.16.128.0 | 172.16.128.1 | 172.16.159.254 | 172.16.159.255    |
| 172.16.160.0 | 172.16.160.1 | 172.16.191.254 | 172.16.191.255    |
| 172.16.192.0 | 172.16.192.1 | 172.16.223.254 | 172.16.223.255    |
| 172.16.224.0 | 172.16.224.1 | 172.16.255.254 | 172.16.255.255    |

## Step 9: Installing Tools on Ubuntu

**Install NFS, CIFS, and iSCSI Tools:**
```bash
sudo apt-get install rpcbind nfs-common cifs-utils smbclient open-iscsi multipath-tools openssh-server alien open-vm-tools open-vm-tools-desktop
```

## Step 10: BIOS/UEFI Settings for Nested Virtualization

1. **Check BIOS/UEFI Settings:**
    - Enable virtualization settings like "Intel VT-x," "Intel Virtualization Technology," or "AMD-V."

2. **Configure Proxmox VM for Nested Virtualization:**
    - In Proxmox web interface, ensure "KVM hardware virtualization" is enabled.
    - Edit VM's config file (`/etc/pve/qemu-server/VMID.conf`):
        ```plaintext
        args: -cpu host,+vmx
        ```

3. **Enable Nested Virtualization:**
    - Add the following line to `/etc/modprobe.d/kvm.conf`:
        ```plaintext
        options kvm_intel nested=1
        ```
    - Reload settings:
        ```sh
        modprobe -r kvm_intel
        modprobe kvm_intel
        ```

4. **Verify VT-x Support:**
    ```sh
    egrep -o '(vmx|svm)' /proc/cpuinfo
    ```

5. **Restart the Proxmox Host.**

## Step 11: Mounting and Testing without Active Directory

**Mount NFS Share:**
```bash
sudo mkdir -p /mnt/vol
sudo mount nfsvol.wina.com:/nfsvol /mnt/vol
df -h
touch /mnt/vol/testfile
ls -la /mnt/vol
rm /mnt/vol/testfile
```

## Step 12: Configuring Ubuntu with Active Directory

1. **Install Required Packages:**
    ```sh
    sudo apt update
    sudo apt install realmd sssd sssd-tools adcli samba-common-bin oddjob oddjob-mkhomedir packagekit nfs-common
    ```

2. **Join the Windows AD Domain:**
    ```sh
    sudo realm discover wina.com
    sudo realm join -U <your_administrator_username> wina.com
    ```

3. **Verify the Join Operation:**
    ```sh
    realm list
    ```

4. **Configure SSSD:**
    Edit `/etc/sssd/sssd.conf`:
    ```bash
    [sssd]
    domains = wina.com
    config_file_version = 2
    services = nss, pam

    [domain/wina.com]
    default_shell = /bin/bash
    krb5_store_password_if_offline = True
    cache_credentials = True
    krb5_realm = WINA.COM
    realmd_tags = manages-system joined-with-adcli
    id_provider = ad
    fallback_homedir = /home/%u@%d
    ad_domain = wina.com
    use_fully_qualified_names = True
    ldap_id_mapping = True
    access_provider = ad
    session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
    ```

5. **Configure PAM for Home Directory Creation:**
    Edit `/etc/pam.d/common-session` and `/etc/pam.d/common-session-noninteractive`:
    ```plaintext
    session required pam_mkhomedir.so skel=/etc/skel umask=0077
    ```

6. **Enable and Start Oddjobd Service:**
    ```sh
    sudo systemctl enable oddjobd
    sudo systemctl start oddjobd
    ```

7. **Restart SSSD and Oddjobd Services:**
    ```sh
    sudo systemctl restart sssd
    sudo systemctl restart oddjobd
    ```

8. **Edit Sudoers File to Allow AD Users to Use Sudo:**
    ```sh
    sudo visudo
    ```
    Add:
    ```plaintext
    %domain\ users@wina.com ALL=(ALL:ALL) ALL
    ```

### Verify the Configuration

1. **Log in as AD User:**
    ```sh
    su - A1@wina.com
    ```

2. **Check Sudo Privileges:**
    ```sh
    sudo -i
    ```

## Step 13: Mounting an NFS Share

1. **Create a Mount Point:**
    ```sh
    sudo mkdir -p /mnt/nfs_share
    ```

2. **Mount the NFS Share:**
    ```sh
    sudo mount -t nfs <nfs_server_ip>:/nfs/share /mnt/nfs_share
    ```

3. **Make the Mount Persistent:**
    ```sh
    sudo nano /etc/fstab
    ```
    Add:
    ```plaintext
    <nfs_server_ip>:/nfs/share /mnt/nfs_share nfs defaults 0 0
    ```

Yes, after configuring your Windows Server 2016 to function as an NTP server, you can then set your ONTAP clusters to synchronize their time with the IP address of this NTP server. Here's how you can set up the NTP server on your ONTAP clusters:

### Configuring ONTAP Cluster to Use NTP Server

1. **Access the ONTAP CLI**:
    - Connect to your ONTAP cluster using SSH or through the console.

2. **Add the NTP Server**:
    - Use the following command to add the NTP server:

    ```shell
    ntp server add -server <NTP_Server_IP>
    ```

    Replace `<NTP_Server_IP>` with the IP address of your Windows Server 2016 that you configured as the NTP server.

3. **Verify the NTP Configuration**:
    - To check the NTP server configuration, use the following command:

    ```shell
    ntp server show
    ```

4. **Enable NTP on the Cluster**:
    - If NTP is not already enabled on the cluster, use the following command to enable it:

    ```shell
    ntp enable
    ```

5. **Verify Time Synchronization**:
    - You can check the synchronization status using:

    ```shell
    ntp status
    ```

### Example Steps

Assuming your NTP server's IP address is `192.168.1.100`:

1. **Add the NTP Server**:

    ```shell
    ntp server add -server 192.168.1.100
    ```

2. **Verify the NTP Server**:

    ```shell
    ntp server show
    ```

3. **Enable NTP (if not already enabled)**:

    ```shell
    ntp enable
    ```

4. **Check the NTP Status**:

    ```shell
    ntp status
    ```

By following these steps, your ONTAP clusters should synchronize their time with the NTP server you configured on your Windows Server 2016, ensuring accurate and consistent timekeeping across your network.
## Network and Machine Details

### Proxmox Node 1 and Node 2

- **Active Directory Address:** `wina.com` and `winb.com`
- **Ubuntu A:** `172.16.96.12` resolves as `ubuntua.wina.com`
- **Windows A:** `172.16.96.10` resolves as `wina.wina.com`
- **Client A:** `172.16.96.11` resolves as `a.wina.com`
- **Ubuntu B:** `172.16.128.12` resolves as `ubuntub.winb.com`
- **Windows B:** `172.16.128.10`
- **Client B:** `172.16.128.11`

### ESXi and vSphere

- **ESXi1:** `172.16.0.10` resolves as `esxi1.wina.com`
- **vSphere in ESXi1:** `172.16.0.11` resolves as `vcsa.wina.com`
- **ESXi2:** `172.16.0.10` resolves as `esxi2.wina.com`
- **vSphere in ESXi2:** `172.16.0.11` resolves as `vcsa.wina.com`

### ONTAP7 NFS Mounts

- **NFS1:** `nfs1.wina.com` with IPs `172.16.0.6-172.16.0.9`
- **NFS2:** `nfs2.wina.com` with IPs `172.16.0.12-172.16.0.16`
### ONTAP7 CIFS Mounts

- CIFS1:** `cifs1.wina.com` with IPs `172.16.0.17-172.16.0.20`
- **CIFS2:** `cifs2.wina.com` with IPs `172.16.0.21-172.16.0.24`

### Proxmox Environment Overview

#### Node 1: Datacenter (myhomelab) > pve

- **C14C1N1 (107)**
- **C14C1N2 (108)**
- **C14C2N1 (109)**
- **C14C2N2 (110)**
- **c7N1 (102)**
- **c7N2 (103)**
- **ESXI1 (117)**
- **ESXI2 (120)**
- **Routerpve (115)**
- **vSwitch (104)**
- **Storage and Network:**
  - local (pve)
  - local-lvm (pve)
  - lvm1 (pve)

#### Node 2: Datacenter (myhomelab) > pve1

- **A (122)**
- **b (116)**
- **ClientA (105)**
- **ClientB (119)**
- **LinuxAPI (121)**
- **Routerpve1 (118)**
- **UbuntuA (111)**
- **UbuntuB (112)**
- **WinA (113)**
- **WinB (114)**
- **Storage and Network:**
  - local (pve1)
  - local-lvm (pve1)
  - lvm1 (pve1)
  - lvm2 (pve1)

### Diagram of Proxmox Lab Environment

Here is a detailed diagram of the Proxmox lab environment setup:

## Additional Resources

- [Proxmox VE Documentation](https://pve.proxmox.com/wiki/Main_Page)
- [VyOS Documentation](https://docs.vyos.io/)
- [NetApp ONTAP Documentation](https://docs.netapp.com/us-en/ontap/index.html)

---

This guide should now serve as a comprehensive teaching tool for students to set up and manage a Proxmox lab environment. It is modular and can be easily updated or forked for improvements and additional information.