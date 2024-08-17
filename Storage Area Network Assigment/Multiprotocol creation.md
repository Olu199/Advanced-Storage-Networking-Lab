### 1. Create the Vserver
This command creates a new Vserver named `Multi_nfs_cif` with an NTFS security style for the root volume.

```bash
vserver create -vserver Multi_nfs_cif -rootvolume Multi_nfs_cif_root -rootvolume-security-style ntfs
```

### 2. Create Network Interfaces
These commands create network interfaces for the Vserver that support both NFS and CIFS protocols.

```bash
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif1 -role data -data-protocol nfs,cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.25 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif2 -role data -data-protocol nfs,cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.26 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif3 -role data -data-protocol nfs,cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.27 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif4 -role data -data-protocol nfs,cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.28 -netmask-length 19
```

### 3. Create a Default Route
This command sets a default route for the Vserver.

```bash
net route create -vserver Multi_nfs_cif -destination 0.0.0.0/0 -gateway 172.16.0.1
```

### 4. Configure DNS and CIFS
These commands configure DNS services and set up CIFS for the Vserver.

```bash
vserver services dns create -vserver Multi_nfs_cif -domain wina.com -name-server 172.16.96.10
vserver cifs create -vserver Multi_nfs_cif -cifs-server Multi_nfs_cif -domain wina.com
```

### 5. Enable NFS on the Vserver
This command enables NFS services for the Vserver.

```bash
vserver nfs create -vserver Multi_nfs_cif
```

### 6. Create UNIX Users
These commands create UNIX users that will be used in the Vserver.

```bash
vserver services unix-user create -vserver Multi_nfs_cif -user u1 -id 1602001613 -primary-gid 1602001606
vserver services unix-user create -vserver Multi_nfs_cif -user u2 -id 1602001612 -primary-gid 1602001606
vserver services unix-user create -vserver Multi_nfs_cif -user w1 -id 1602001611 -primary-gid 1602001609
vserver services unix-user create -vserver Multi_nfs_cif -user w2 -id 1602001610 -primary-gid 1602001609
```

### 7. Modify CIFS Options
These commands modify CIFS options to disable the mapping of administrative users to the root.

```bash
set advanced
vserver cifs options modify -vserver Multi_nfs_cif -is-admin-users-mapped-to-root-enabled false
set admin
```

### 8. Create and Modify Volumes
These commands create volumes for Windows and UNIX file systems, modify their security styles, and set permissions.

```bash
volume create -vserver Multi_nfs_cif -aggregate aggr2_ontap7_01_fcal -volume win_vol -size 50MB -junction-path /win_vol
volume create -vserver Multi_nfs_cif -aggregate aggr3_ontap7_01_fcal -volume unix_vol -size 50MB -junction-path /unix_vol
volume modify -vserver Multi_nfs_cif -volume unix_vol -security-style unix -unix-permissions 0777
```

### 9. Create and Modify Export Policies
These commands create an export policy and modify volumes to apply the export policy.

```bash
export-policy create -vserver Multi_nfs_cif -policyname Multi_nfs_cif
vserver export-policy rule create -vserver Multi_nfs_cif -policyname Multi_nfs_cif -ruleindex 1 -protocol nfs,cifs -clientmatch 172.16.96.12 -rorule any -rwrule any -superuser any
volume modify -policy Multi_nfs_cif -vserver Multi_nfs_cif -volume unix_vol,win_vol,Multi_nfs_cif_root
```

### 10. Create CIFS Shares
These commands create CIFS shares for the volumes and manage access control.

```bash
vserver cifs share create -vserver Multi_nfs_cif -share-name win_vol -path /win_vol
vserver cifs share create -vserver Multi_nfs_cif -share-name unix_vol -path /unix_vol
vserver cifs share access-control delete -vserver Multi_nfs_cif -share win_vol -user-or-group Everyone
vserver cifs share access-control delete -vserver Multi_nfs_cif -share unix_vol -user-or-group Everyone
vserver cifs share access-control create -vserver Multi_nfs_cif -share unix_vol -user-or-group "Shared Files" -user-group-type windows -permission Full_Control
vserver cifs share access-control create -vserver Multi_nfs_cif -share win_vol -user-or-group "Shared Files" -user-group-type windows  -permission Full_Control
vserver cifs share show -vserver Multi_nfs_cif
```

### 11. Create Name Mappings
These commands create name mappings between Windows and UNIX users.

```bash
vserver name-mapping create -vserver Multi_nfs_cif -direction win-unix -position 1 -pattern "wina\\(.+)" -replacement "\1"
vserver name-mapping create -vserver Multi_nfs_cif -direction unix-win  -position 1 -pattern "wina\\(.+)" -replacement "\1"
```
