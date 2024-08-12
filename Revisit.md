
vserver create -vserver Multi_nfs_cif -rootvolume Multi_nfs_cif_root -rootvolume-security-style ntfs
vserver create -vserver Multi_nfs_cif -rootvolume Multi_nfs_cif_root -rootvolume-security-style unix






network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.25 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif2 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.26 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif3 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.27 -netmask-length 19
network interface create -vserver Multi_nfs_cif -lif Multi_nfs_cifs_lif4 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.28 -netmask-length 19
net route create -vserver Multi_nfs_cif -destination 0.0.0.0/0 -gateway 172.16.0.1





vserver services dns create -vserver Multi_nfs_cif -domain wina.com -name-server 172.16.96.10
vserver cifs create -vserver Multi_nfs_cif -cifs-server Multi_nfs_cif -domain wina.com



volume create -vserver Multi_nfs_cif -aggregate aggr2_ontap7_01_fcal -volume home_cifs_vol1 -size 50MB -junction-path /home_cifs_vol1 
volume create -vserver Multi_nfs_cif -aggregate aggr3_ontap7_01_fcal -volume home_cifs_vol2 -size 50MB -junction-path /home_cifs_vol2 


vserver cifs share create -vserver Multi_nfs_cif -share-name home_cifs_vol1 -path /home_cifs_vol1
vserver cifs share create -vserver Multi_nfs_cif -share-name home_cifs_vol2 -path /home_cifs_vol2


vserver cifs share access-control create -share home_cifs_vol1 -user-or-group "Home Dir Users" -permission Full_Control -vserver Multi_nfs_cif
vserver cifs share access-control create -share home_cifs_vol2 -user-or-group "Home Dir Users" -permission Full_Control -vserver Multi_nfs_cif


vserver cifs share access-control delete -share home_cifs_vol1 -user-or-group Everyone -vserver Multi_nfs_cif
vserver cifs share access-control delete -share home_cifs_vol2 -user-or-group Everyone -vserver Multi_nfs_cif


vserver cifs home-directory search-path add -vserver Multi_nfs_cif -path /home_cifs_vol1
vserver cifs home-directory search-path add -vserver Multi_nfs_cif -path /home_cifs_vol2

vserver cifs share create -vserver Multi_nfs_cif -share-name %w -path %w -share-properties homedirectory

 \\multi_hd_cifs.wina.com\home_cifs_vol1