### 1. Edit iSCSI Configuration
First, you need to edit the iSCSI configuration file.

```bash
sudo nano /etc/iscsi/iscsid.conf
```

**Explanation:**
- **`sudo nano /etc/iscsi/iscsid.conf`:** Opens the iSCSI configuration file in the `nano` text editor with elevated privileges.

Inside the file, you will set the iSCSI node startup to automatic, configure CHAP authentication, and specify the credentials.

```bash
node.startup = automatic
node.session.auth.authmethod = CHAP
node.session.auth.username = <initiator_iqn>
node.session.auth.password = <initiator_password>
node.session.auth.username_in = <target_iqn>
node.session.auth.password_in = <target_password>
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = <initiator_iqn>
discovery.sendtargets.auth.password = <initiator_password>
discovery.sendtargets.auth.username_in = <target_iqn>
discovery.sendtargets.auth.password_in = <target_password>
```

**Explanation:**
- **`node.startup = automatic`:** Sets the iSCSI node to start automatically on boot.
- **`node.session.auth.authmethod = CHAP`:** Specifies that CHAP authentication will be used.
- **`node.session.auth.username` and `node.session.auth.password`:** Replace `<initiator_iqn>` with the iSCSI Qualified Name (IQN) of your initiator and `<initiator_password>` with your chosen password.
- **`node.session.auth.username_in` and `node.session.auth.password_in`:** Replace `<target_iqn>` with the IQN of the target (e.g., a NetApp device) and `<target_password>` with the associated password.

### 2. Comment Out Manual Node Startup

```bash
# node.startup = manual
```

**Explanation:**
- **Commenting Out:** The `node.startup = manual` line is commented out to ensure that the automatic startup setting takes precedence.

### 3. Retrieve IQN and Passwords

To retrieve the initiator and target IQNs:

```bash
sudo cat /etc/iscsi/initiatorname.iscsi
```

```bash
vserver iscsi show
```

**Explanation:**
- **`sudo cat /etc/iscsi/initiatorname.iscsi`:** Displays the IQN of the iSCSI initiator.
- **`vserver iscsi show`:** Command to display the target IQN for the NetApp device.

Passwords `<initiator_password>` and `<target_password>` should be set according to your configuration.

### 4. iSCSI Discovery and Login

```bash
sudo iscsiadm -m discovery -t st -p <target_ip>:3260,1000 <target_iqn>
```

```bash
sudo iscsiadm -m node –-targetname <target_iqn> -l
```

**Explanation:**
- **`iscsiadm -m discovery`:** Discovers available iSCSI targets on the specified IP address and port.
- **`iscsiadm -m node`:** Logs in to the specified iSCSI target.

### 5. Verify iSCSI Session

```bash
sudo iscsiadm -m session
vserver iscsi connection show
```

**Explanation:**
- **`iscsiadm -m session`:** Displays the current iSCSI sessions.
- **`vserver iscsi connection show`:** Shows active iSCSI connections on the NetApp device.

### 6. Install and Configure Multipath Tools

Update the system and install `multipath-tools`.

```bash
sudo apt-get update
sudo apt-get install multipath-tools
```

Verify multipath setup:

```bash
sudo multipath -ll
```

**Explanation:**
- **`multipath -ll`:** Lists all multipath devices and their configurations.

Create and edit the multipath configuration file:

```bash
sudo nano /etc/multipath.conf
```

Add the following configuration:

```bash
defaults {
        user_friendly_names no
}
blacklist {
         devnode "sda"
}
multipaths {
         multipath {
                 wwid <wwid>
                 alias lun1
         }
}
```

**Explanation:**
- **`user_friendly_names no`:** Disables friendly names for multipath devices.
- **`blacklist`:** Excludes the specified device node (e.g., "sda") from multipath.
- **`multipath`:** Configures a multipath device using the WWID and assigns it an alias.

### 7. Restart Services

```bash
sudo /etc/init.d/open-iscsi restart 
sudo /etc/init.d/multipath-tools restart
# OR
sudo systemctl restart multipath-tools
```

**Explanation:**
- **`restart`:** Restarts the iSCSI and multipath services to apply the changes.

### 8. Verify and Manage Multipath Device

```bash
sudo iscsiadm -m session
sudo multipath -ll
sudo fdisk -l
```

**Explanation:**
- **`fdisk -l`:** Lists the available disk partitions.

### 9. Partition, Format, and Mount

```bash
sudo fdisk /dev/mapper/lun1
sudo mkfs -t ext3 /dev/mapper/lun1-part1
sudo mkdir /mnt/lun1
sudo mount -t ext3 /dev/mapper/lun1-part1 /mnt/lun1
sudo df -h
```

**Explanation:**
- **`fdisk`:** Used to create partitions.
- **`mkfs -t ext3`:** Formats the partition with the `ext3` filesystem.
- **`mkdir` and `mount`:** Creates a directory and mounts the partition.
- **`df -h`:** Displays disk space usage.

### 10. Test Multipath

```bash
cd /mnt/lun1
ls
sudo dd if=/dev/zero of=file1 bs=4K count=5000
ls
sudo iscsiadm -m session -s
```

**Explanation:**
- **`dd if=/dev/zero`:** Writes a test file to the mounted partition to verify the multipath setup.
- **`iscsiadm -m session -s`:** Displays session statistics.

### 11. Edit fstab for Persistent Mounts

Finally, edit `/etc/fstab` to ensure the LUN is mounted persistently across reboots.

```bash
sudo nano /etc/fstab
```

Add the following line to `/etc/fstab`:

```bash
/dev/mapper/lun1-part1  /mnt/lun1  ext3  defaults  0  0
```

**Explanation:**
- **`/etc/fstab`:** The configuration file that defines how disk partitions and devices are mounted on startup.
