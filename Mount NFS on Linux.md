### SMB IP to FQDN Resolution

For each vServer/SMB IP, you can obtain the Fully Qualified Domain Name (FQDN) from the Windows administrator. Alternatively, you can use the `nslookup` command to resolve the FQDN directly from one of the IPs. Here’s how:

**Example Command:**
```bash
nslookup 172.16.0.6
```

**Example Output:**
```
Server:  dns.server.address
Address:  dns.server.address#53

Name:    ford.motors.com
Address: 10.2.10.250
```

### Linux Side: Mounting NFS Shares

To mount an NFS share on your Linux system, follow these steps:

1. **Create a Mount Directory**  
   First, create a directory where the NFS share will be mounted.

   **Command:**
   ```bash
   sudo mkdir /mnt/nfs_dir_vol_a
   ```

2. **Mount the NFS Share**  
   Next, mount the NFS share using the FQDN obtained earlier.

   **Command:**
   ```bash
   sudo mount ford.motors.com:/nfs_vol_a /mnt/nfs_dir_vol_a
   ```

