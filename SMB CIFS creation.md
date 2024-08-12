
vserver create -vserver cifsa -rootvolume cifs_root -rootvolume-security-style ntfs
vserver create -vserver cifsb -rootvolume cifs_root -rootvolume-security-style ntfs





network interface create -vserver cifsa -lif cifsa_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0g -address 172.16.0.14 -netmask-length 19
network interface create -vserver cifsa -lif cifsa_lif2 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0h -address 172.16.0.15 -netmask-length 19
network interface create -vserver cifsb -lif cifsb_lif1 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0g -address 172.16.0.16 -netmask-length 19
network interface create -vserver cifsb -lif cifsb_lif2 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0h -address 172.16.0.17 -netmask-length 19
net route create -vserver cifsa -destination 0.0.0.0/0 -gateway 172.16.0.1
net route create -vserver cifsb -destination 0.0.0.0/0 -gateway 172.16.0.1





vserver services dns create -vserver cifsa -domains wina.com -name-servers 172.16.96.10
vserver services dns create -vserver cifsb -domains winb.com -name-servers 172.16.128.10

vserver cifs create -vserver cifsa -cifs-server cifsa -domain wina.com
ww



volume create -vserver cifsa -aggregate aggr1_ontap7_02_fcal -volume cifs_vola -size 50MB -junction-path /cifs_vola
volume create -vserver cifsb -aggregate aggr1_ontap7_02_fcal -volume cifs_volb -size 50MB -junction-path /cifs_volb

vserver cifs share create -vserver cifsa -share-name cifs_vola -path /cifs_vola
vserver cifs share create -vserver cifsb -share-name cifs_volb -path /cifs_volb


vserver cifs share access-control delete -share cifs_vola -user-or-group Everyone
vserver cifs share access-control delete -share cifs_volb -user-or-group Everyone


vserver cifs share access-control create -share cifs_vola -user-or-group "Windows Users A" -user-group-type windows -permission Full_Control -vserver cifsa
vserver cifs share access-control create -share cifs_volb -user-or-group "Windows Users B" -user-group-type windows -permission Full_Control -vserver cifsb



