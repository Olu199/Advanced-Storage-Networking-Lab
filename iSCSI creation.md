sudo apt-get update
sudo apt-get install open-iscsi
sudo systemctl start iscsid
sudo systemctl enable iscsid
sudo cat /etc/iscsi/initiatorname.iscsi


iqn.1991-05.com.microsoft:wina.wina.com
iqn.1991-05.com.microsoft:a.wina.com


iqn.2004-10.com.ubuntu:01:8327b95201e
Target Name: iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24

sudo cat /etc/iscsi/initiatorname.iscsi


vserver create -vserver iSCSI

network port broadcast-domain create -ipspace Default -broadcast-domain FC_FabricA -mtu 1500
network port broadcast-domain create -ipspace Default -broadcast-domain FC_FabricB -mtu 1500
network port broadcast-domain add-ports -broadcast-domain FC_FabricA -ports ontap7-01:e0i,ontap7-02:e0i
network port broadcast-domain add-ports -broadcast-domain FC_FabricB -ports ontap7-01:e0j,ontap7-02:e0j 



network subnet create -ipspace Default -subnet-name FC_FabricA -broadcast-domain FC_FabricA -subnet 172.16.0.0/255.255.224.0 -gateway 172.16.0.1 -ip-ranges 172.16.0.29-172.16.0.37
network subnet create -ipspace Default -subnet-name FC_FabricB -broadcast-domain FC_FabricB -subnet 172.16.32.0/255.255.224.0 -gateway 172.16.32.1 -ip-ranges 172.16.32.29-172.16.32.37


network interface create -vserver iSCSI -lif iSCSIs_node1a -role data -data-protocol iscsi -home-node ontap7-01 -home-port e0i -subnet-name FC_FabricA
network interface create -vserver iSCSI -lif iSCSIs_node2a -role data -data-protocol iscsi -home-node ontap7-02 -home-port e0i -subnet-name FC_FabricA
network interface create -vserver iSCSI -lif iSCSIs_node1b -role data -data-protocol iscsi -home-node ontap7-01 -home-port e0j -subnet-name FC_FabricB
network interface create -vserver iSCSI -lif iSCSIs_node2b -role data -data-protocol iscsi -home-node ontap7-02 -home-port e0j -subnet-name FC_FabricB

lun portset create -vserver iSCSI -portset iSCSI_port_i -protocol iscsi -port-name iSCSIs_node1a,iSCSIs_node2a

lun portset create -vserver iSCSI -portset iSCSI_port_j -protocol iscsi -port-name iSCSIs_node1b,iSCSIs_node2b


lun igroup create -vserver iSCSI -igroup Win_client_wina -protocol iscsi -ostype windows iqn.1991-05.com.microsoft:wina.wina.com
lun igroup create -vserver iSCSI -igroup Win_client_a -protocol iscsi -ostype windows iqn.1991-05.com.microsoft:a.wina.com
lun igroup create -vserver iSCSI -igroup Linux_client -protocol iscsi -ostype linux iqn.2004-10.com.ubuntu:01:8327b95201e

volume create -vserver iSCSI -aggregate aggr2_ontap7_01_fcal -volume iscsi_win_vol1 -size 110MB
volume create -vserver iSCSI -aggregate aggr2_ontap7_01_fcal -volume iscsi_win_vol2 -size 110MB 
volume create -vserver iSCSI -aggregate aggr2_ontap7_01_fcal -volume iscsi_linux_vol1 -size 110MB

lun create -vserver iSCSI -volume iscsi_win_vol1 -lun iscsi_win_vol1 -size 100mb -ostype windows_2008dd
lun create -vserver iSCSI -volume iscsi_win_vol2 -lun iscsi_win_vol2 -size 100mb -ostype windows_2008
lun create -vserver iSCSI -volume iscsi_linux_vol1 -lun iscsi_linux_vol1 -size 100mb -ostype linux
 
lun map  -vserver iSCSI  -volume iscsi_win_vol1 -lun iscsi_win_vol1 -igroup Win_client_wina
lun map  -vserver iSCSI  -volume iscsi_win_vol2 -lun iscsi_win_vol2 -igroup Win_client_a
lun map  -vserver iSCSI  -volume iscsi_linux_vol1 -lun iscsi_linux_vol1 -igroup Linux_client 

vserver iscsi show



vserver iscsi security create -vserver iSCSI -initiator-name iqn.1991-05.com.microsoft:a.wina.com -auth-type CHAP -user-name iqn.1991-05.com.microsoft:a.wina.com -outbound-user-name iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7

vserver iscsi security create -vserver iSCSI -initiator-name iqn.1991-05.com.microsoft:wina.wina.com -auth-type CHAP -user-name iqn.1991-05.com.microsoft:wina.wina.com -outbound-user-name iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7

vserver iscsi security create -vserver iSCSI -initiator-name iqn.2004-10.com.ubuntu:01:8327b95201e -auth-type CHAP -user-name iqn.2004-10.com.ubuntu:01:8327b95201e -outbound-user-name iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7

inbound from window to this svm storage
    password: 2723damy1234
svm outpound form storage to the window
outbound:  2723damy12345






