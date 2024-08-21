```
systemctl stop ceph-mon.target
systemctl stop ceph-mgr.target
systemctl stop ceph-mds.target
systemctl stop ceph-osd.target
```
```
rm -rf /etc/systemd/system/ceph*
rm -rf /var/lib/ceph/mon/
rm -rf /var/lib/ceph/mgr/
rm -rf /var/lib/ceph/mds/
```
```
apt -y purge ceph-mon ceph-osd ceph-mgr ceph-mds
```
```
reboot
```
