vserver create -vserver Multi_nfs_cif -rootvolume Multi_nfs_cif_root -rootvolume-security-style ntfs


network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif1 -role data -data-protocol nfs,cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.25 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif2 -role data -data-protocol nfs,cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.26 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif3 -role data -data-protocol nfs,cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.27 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif4 -role data -data-protocol nfs,cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.28 -netmask-length 19
net route create -vserver Multi_nfs_cif -destination 0.0.0.0/0 -gateway 172.16.0.1
 

vserver services dns create -vserver Multi_nfs_cif -domain wina.com -name-server 172.16.96.10
vserver cifs create -vserver Multi_nfs_cif -cifs-server Multi_nfs_cif -domain wina.com

vserver nfs create -vserver Multi_nfs_cif

vserver services unix-user create -vserver Multi_nfs_cif -user u1 -id 1602001613 -primary-gid 1602001606
vserver services unix-user create -vserver Multi_nfs_cif -user u2 -id 1602001612 -primary-gid 1602001606
vserver services unix-user create -vserver Multi_nfs_cif -user w1 -id 1602001611 -primary-gid 1602001609
vserver services unix-user create -vserver Multi_nfs_cif -user w2 -id 1602001610 -primary-gid 1602001609



set advanced

vserver cifs options modify -vserver Multi_nfs_cif -is-admin-users-mapped-to-root-enabled false
set admin


volume create -vserver Multi_nfs_cif -aggregate aggr2_ontap7_01_fcal -volume win_vol -size 50MB -junction-path /win_vol

volume create -vserver Multi_nfs_cif -aggregate aggr3_ontap7_01_fcal -volume unix_vol -size 50MB -junction-path /unix_vol

volume modify -vserver Multi_nfs_cif -volume unix_vol -security-style unix -unix-permissions 0777


export-policy create -vserver Multi_nfs_cif -policyname Multi_nfs_cif
vserver export-policy rule create -vserver Multi_nfs_cif -policyname Multi_nfs_cif -ruleindex 1 -protocol nfs,cifs -clientmatch 172.16.96.12 -rorule any -rwrule any -superuser any

volume modify -policy Multi_nfs_cif -vserver Multi_nfs_cif -volume unix_vol,win_vol,Multi_nfs_cif_root


vserver cifs share create -vserver Multi_nfs_cif -share-name win_vol -path /win_vol
vserver cifs share create -vserver Multi_nfs_cif -share-name unix_vol -path /unix_vol

vserver cifs share access-control delete -vserver Multi_nfs_cif -share win_vol -user-or-group Everyone
vserver cifs share access-control delete -vserver Multi_nfs_cif -share unix_vol -user-or-group Everyone
vserver cifs share access-control create -vserver Multi_nfs_cif -share unix_vol -user-or-group "Shared Files" -user-group-type windows -permission Full_Control
vserver cifs share access-control create -vserver Multi_nfs_cif -share win_vol -user-or-group "Shared Files" -user-group-type windows  -permission Full_Control
vserver cifs share show -vserver Multi_nfs_cif
  
vserver name-mapping create -vserver Multi_nfs_cif -direction win-unix -position 1 -pattern "wina\\(.+)" -replacement "\1"
vserver name-mapping create -vserver Multi_nfs_cif -direction unix-win  -position 1 -pattern "wina\\(.+)" -replacement "\1"

