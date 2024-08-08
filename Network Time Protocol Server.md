 d### Steps to Configure Windows Server 2016 as an NTP Server

1. **Open the Command Prompt as Administrator:**
   - Press `Win + X` and select "Command Prompt (Admin)" or "Windows PowerShell (Admin)".

2. **Configure the Windows Time Service:**
   - Enter the following commands to configure the Windows Time service to act as an NTP server.

```shell
w32tm /config /manualpeerlist:"0.pool.ntp.org,1.pool.ntp.org,2.pool.ntp.org" /syncfromflags:manual /reliable:YES /update
net stop w32time
net start w32time
```
   - Replace the NTP servers (`0.pool.ntp.org,1.pool.ntp.org,2.pool.ntp.org`) with the ones you prefer to use.

3. **Open the Registry Editor:**
   - Press `Win + R`, type `regedit`, and press Enter.

4. **Edit the Registry to Enable NTP Server:**
   - Navigate to the following key:
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config
     ```
   - Find the entry named `AnnounceFlags` and set its value to `5`.

5. **Configure NTP Server Setting:**
   - Navigate to:
     ```
     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer
     ```
   - Find the entry named `Enabled` and set its value to `1`.

6. **Restart the Windows Time Service:**
   - To apply the changes, restart the Windows Time service with the following commands:

```shell
net stop w32time
net start w32time
```

### Verify NTP Server Configuration

To verify that your server is functioning as an NTP server, you can use the `w32tm` command:

```shell
w32tm /query /status
```

Additionally, you can check the server from a client machine to ensure it can synchronize time from your NTP server.

### Firewall Configuration

Ensure that your firewall settings allow NTP traffic (UDP port 123) so that the server can communicate with other devices on the network.

By following these steps, you should be able to configure your Windows Server 2016 to act as an NTP server, providing accurate time synchronization for your network devices.




In Ontap Set NTP Server like this

ntp server create -server 172.16.192.2

set d
cluster time-service ntp status show
