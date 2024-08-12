vserver create -vserver nfs -rootvolume nfs_root -rootvolume-security-style unix

vserver nfs create -vserver nfs

vserver export-policy create -vserver nfs -policyname nfs_vol
vserver export-policy create -vserver nfs -policyname nfs_vol_q01

vserver export-policy rule create -vserver nfs -policyname nfs_vol -ruleindex 1 -protocol nfs -clientmatch 172.16.96.12,172.16.128.12 -rorule any -rwrule never
vserver export-policy rule create -vserver nfs -policyname nfs_vol_q01 -ruleindex 1 -protocol nfs -clientmatch 172.16.96.12,172.16.128.12 -rorule any -rwrule any

network interface create -vserver nfs -lif nfs_lif1 -role data -data-protocol nfs -home-node ontap7-01 -home-port e0e -address 172.16.0.6 -netmask-length 19
network interface create -vserver nfs -lif nfs_lif2 -role data -data-protocol nfs -home-node ontap7-02 -home-port e0f -address 172.16.0.7 -netmask-length 19
network interface create -vserver nfs -lif nfs_lif3 -role data -data-protocol nfs -home-node ontap7-01 -home-port e0e -address 172.16.0.8 -netmask-length 19
network interface create -vserver nfs -lif nfs_lif4 -role data -data-protocol nfs -home-node ontap7-02 -home-port e0f -address 172.16.0.9 -netmask-length 19
net route create -vserver nfs -destination 0.0.0.0/0 -gateway 172.16.0.1

volume create -vserver nfs -aggregate aggr1_ontap7_01_fcal -volume nfs_vol_a -size 50MB -junction-path /nfs_vol_a -security-style unix -policy nfs_vol -unix-permissions 755
volume create -vserver nfs -aggregate aggr1_ontap7_01_fcal -volume nfs_vol_b-size 50MB -junction-path /nfs_vol_b-security-style unix -policy nfs_vol -unix-permissions 755

vol modify -vserver nfs -volume nfs_root -policy nfs_vol
qtree create -volume nfs_vol_a -qtree q01 -security-style unix -oplock-mode enable -unix-permissions ---rwxr-xr-x -vserver nfs -export-policy nfs_vol_q01
qtree create -volume nfs_vol_b-qtree q01 -security-style unix -oplock-mode enable -unix-permissions ---rwxr-xr-x -vserver nfs -export-policy nfs_vol_q01

vserver services unix-user create -user ubuntua -id 1000 -primary-gid 1000

vserver export-policy check-access -vserver nfs -volume nfs_vol_a -client-ip 172.16.96.12 -authentication-method none -protocol nfs3 -access-type read-write -qtree q01

