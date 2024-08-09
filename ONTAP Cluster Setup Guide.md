## Follow this to get started: [link](https://github.com/MRCzap/ontapsimulator)



### 1. Convert the Node to a Template
Convert the VM to a template before you boot. This ensures you can create multiple node instances without repeating the entire setup process, making your cluster setup a lot easier. Add these network interfaces `vmbr2` to the template:

| Node   | Network Device | MAC Address             | Bridge | Model       |
| ------ | -------------- | ----------------------- | ------ | ----------- |
| Node # | net0           | e1000-BC:24:11:E1:01:21 | vmbr0  | Intel E1000 |
| Node # | net1           | e1000-BC:24:11:F7:BC:71 | vmbr1  | Intel E1000 |
| Node # | net2           | e1000-BC:24:11:5C:64:76 | vmbr2  | Intel E1000 |
| Node # | net3           | e1000-BC:24:11:C6:C9:B8 | vmbr2  | Intel E1000 |
| Node # | net4           | e1000-BC:24:11:04:1D:A3 | vmbr2  | Intel E1000 |
| Node # | net5           | e1000-BC:24:11:A3:01:52 | vmbr2  | Intel E1000 |
| Node # | net6           | e1000-BC:24:11:F5:EE:89 | vmbr2  | Intel E1000 |
| Node # | net7           | e1000-BC:24:11:0C:C1:F2 | vmbr2  | Intel E1000 |
| Node # | net8           | e1000-BC:24:11:9D:1C:20 | vmbr2  | Intel E1000 |
| Node # | net9           | e1000-BC:24:11:B8:A4:FC | vmbr2  | Intel E1000 |
| Node # | net10          | e1000-BC:24:11:31:3F:28 | vmbr2  | Intel E1000 |
| Node # | net11          | e1000-BC:24:11:22:EC:01 | vmbr2  | Intel E1000 |
| Node # | net12          | e1000-BC:24:11:1B:7D:F0 | vmbr2  | Intel E1000 |
| Node # | net13          | e1000-BC:24:11:79:D0:10 | vmbr2  | Intel E1000 |

### 2. Create Full Clones from the Template
You will need to make two full clones from the template:

![image](https://github.com/user-attachments/assets/abdd5f5f-03a7-43dc-9e13-16d999f1c448)
- It will be a full clone:

  ![image](https://github.com/user-attachments/assets/d71d4aad-10d7-4133-b8a6-fc8439e998a2)
- Make sure to name it and select the mode `Full Clone`.

### 3. Start Both Nodes
- Interrupt the second node to change the serial number so that you can pair both nodes as a cluster.
- Power on the second node for the cluster, and when you see `Hit [Enter] to boot ...`, hit the space bar to load the `vloader`:

![image](https://github.com/user-attachments/assets/538b4ba3-2ed4-407d-a5c7-a7c246088030)

I like to use proxmox to access the vloader and here is how:



```bash
root@pve1:~# qm set 125 -serial0 socket
update VM 125: -serial0 socket
root@pve1:~# qm terminal 125
starting serial terminal on interface serial0 (press Ctrl+O to exit)
VLOADER>
```
At the VLOADER> prompt, enter the following commands:

```bash
set console="comconsole,vidconsole"
set comconsole_speed=115200
boot
```
```bash
set console="comconsole,vidconsole"
```
```bash
set comconsole_speed=115200
```
```bash
qm set 125 -serial0 socket
```
```bash
qm terminal 125
```

When you see the normal boot process for node 2, change the Serial Number and System ID for this node:

```bash
setenv SYS_SERIAL_NUM 4034389-06-2
```
```bash
setenv bootarg.nvram.sysid 4034389062
```
```bash
printenv SYS_SERIAL_NUM
```
```bash
printenv bootarg.nvram.sysid
```
```bash
boot
```


### Deployment
After completing the above steps for each node, deploy ONTAP using the provided GitHub link.

### Very Important Notes
1. **Proxmox Backup is not reliable, so use snapshots to protect the ONTAP VM. Take snapshots whenever you are making changes to the VM that are hotplug related. Then power off the VM before you make the additional or removal of a virtual device.**

   ![image](https://github.com/user-attachments/assets/397f6a6b-c4d3-4c00-acec-cb4525ea6767)
   ![image](https://user-images.githubusercontent.com/115875629/208877343-6e64c962-7323-46d4-a899-2689f4b6aef1.png)
   ![image](https://user-images.githubusercontent.com/115875629/208877560-6fbf7fff-f0cd-4de4-bda3-978a52a13413.png)

