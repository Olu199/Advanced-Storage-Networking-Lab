# IMPORTANT: Everying inside arrows '<>' is a variable which must be replaced #

sudo nano /etc/iscsi/iscsid.conf

 node.startup = automatic
 node.session.auth.authmethod = CHAP
 node.session.auth.username = iqn.2004-10.com.ubuntu:01:8327b95201e
 node.session.auth.password = 2723damy1234
 node.session.auth.username_in = iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24
 node.session.auth.password_in = 2723damy12345
 discovery.sendtargets.auth.authmethod = CHAP
 discovery.sendtargets.auth.username = iqn.2004-10.com.ubuntu:01:8327b95201e
 discovery.sendtargets.auth.password = 2723damy1234
 discovery.sendtargets.auth.username_in = iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24
 discovery.sendtargets.auth.password_in = 2723damy12345

Comment out:
node.startup = manual

To get iqn.2004-10.com.ubuntu:01:8327b95201e = sudo cat /etc/iscsi/initiatorname.iscsi
To get iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24 = vserver iscsi show
2723damy1234 = 
2723damy12345 = 

================================================================

Discovery:
sudo iscsiadm -m discovery -t st -p 172.16.0.29:3260,1000 iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24

Login:
sudo iscsiadm -m node –-targetname iqn.1992-08.com.netapp:sn.5a3d4e7550f011efad18bc2411c680a7:vs.24 -l

Verify:
sudo iscsiadm -m session
vserver iscsi connection show

=================================================================

sudo apt-get update
sudo apt-get install multipath-tools

sudo multipath -ll
Note wwid at start of output eg 3600a09807770457a795d486b3232646c = 3600a09807770457a795d577573357553 

sudo nano /etc/multipath.conf

defaults {
        user_friendly_names no
}
blacklist {
         devnode "sda"
}
multipaths {
         multipath {
                 wwid 3600a09807770457a795d577573357553 
                 alias lun1
         }
}


====================================================================

Restart services:
sudo /etc/init.d/open-iscsi restart 
sudo /etc/init.d/multipath-tools restart
or sudo systemctl restart multipath-tools


Verify:
sudo iscsiadm -m session
sudo multipath -ll
sudo fdisk -l

cd /opt/netapp/santools/
sudo sanlun lun show -p

Partition, format, mount and verify:
sudo fdisk /dev/mapper/lun1
sudo mkfs -t ext3 /dev/mapper/lun1-part1
sudo mkdir /mnt/lun1
sudo mount -t ext3 /dev/mapper/lun1-part1 /mnt/lun1
sudo df -h

Test multipath:
cd /mnt/lun1
ls
sudo dd if=/dev/zero of=file1 bs=4K count=5000
ls
sudo iscsiadm -m session -s

Edit fstab to make mount persistent


