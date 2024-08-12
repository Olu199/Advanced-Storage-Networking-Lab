
vserver create -vserver Home_Dir -rootvolume Home_Dir_root -rootvolume-security-style ntfs





network interface create -vserver Home_Dir -lif Home_Dir_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.21 -netmask-length 19
network interface create -vserver Home_Dir -lif Home_Dir_lif2 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.22 -netmask-length 19
network interface create -vserver Home_Dir -lif Home_Dir_lif3 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.23 -netmask-length 19
network interface create -vserver Home_Dir -lif Home_Dir_lif4 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.24 -netmask-length 19
net route create -vserver Home_Dir -destination 0.0.0.0/0 -gateway 172.16.0.1





vserver services dns create -vserver Home_Dir -domain wina.com -name-server 172.16.96.10
vserver cifs create -vserver Home_Dir -cifs-server Home_Dir -domain wina.com



volume create -vserver Home_Dir -aggregate aggr2_ontap7_01_fcal -volume home_cifs_vol1 -size 50MB -junction-path /home_cifs_vol1 
volume create -vserver Home_Dir -aggregate aggr3_ontap7_01_fcal -volume home_cifs_vol2 -size 50MB -junction-path /home_cifs_vol2 


vserver cifs share create -vserver Home_Dir -share-name home_cifs_vol1 -path /home_cifs_vol1
vserver cifs share create -vserver Home_Dir -share-name home_cifs_vol2 -path /home_cifs_vol2


vserver cifs share access-control create -share home_cifs_vol1 -user-or-group "Home Dir Users" -permission Full_Control -vserver Home_Dir
vserver cifs share access-control create -share home_cifs_vol2 -user-or-group "Home Dir Users" -permission Full_Control -vserver Home_Dir


vserver cifs share access-control delete -share home_cifs_vol1 -user-or-group Everyone -vserver Home_Dir
vserver cifs share access-control delete -share home_cifs_vol2 -user-or-group Everyone -vserver Home_Dir


vserver cifs home-directory search-path add -vserver Home_Dir -path /home_cifs_vol1
vserver cifs home-directory search-path add -vserver Home_Dir -path /home_cifs_vol2

vserver cifs share create -vserver Home_Dir -share-name %w -path %w -share-properties homedirectory

 \\multi_hd_cifs.wina.com\home_cifs_vol1