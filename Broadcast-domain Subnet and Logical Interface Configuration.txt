broadcast-domain create -broadcast-domain NAS -mtu 1500
broadcast-domain create -broadcast-domain SAN -mtu 1500

broadcast-domain add-ports -broadcast-domain NAS -ports ontap7-01:e0g,ontap7-01:e0h,ontap7-02:e0g,ontap7-02:e0h,ontap7-01:e0i,ontap7-01:e0j,ontap7-01:e0k,ontap7-01:e0l,ontap7-02:e0i,ontap7-02:e0j,ontap7-02:e0k,ontap7-02:e0l
broadcast-domain add-ports -broadcast-domain SAN -ports ontap7-01:e0m,ontap7-01:e0n,ontap7-02:e0m,ontap7-02:e0n

subnet create -subnet-name Nfs -broadcast-domain Nfs -subnet 172.16.0.0/19 -ip-ranges 172.16.0.6-172.16.0.10 -gateway 172.16.0.1
subnet create -subnet-name SAN -broadcast-domain SAN -subnet 172.16.64.0/19 -ip-ranges 172.16.64.6-172.16.64.14 -gateway 172.16.64.1


network interface create -vserver iscsi -lif iscsi_lif1 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0m -subnet-name SAN
network interface create -vserver iscsi -lif iscsi_lif2 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0m -subnet-name SAN
network interface create -vserver iscsi -lif iscsi_lif3 -role data -data-protocol cifs -home-node ontap7-01 -home-port e0n -subnet-name SAN
network interface create -vserver iscsi -lif iscsi_lif4 -role data -data-protocol cifs -home-node ontap7-02 -home-port e0n -subnet-name SAN
net route create -vserver iscsi -destination 0.0.0.0/0 -gateway 172.16.64.1


