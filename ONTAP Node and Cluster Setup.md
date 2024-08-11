Certainly! Here’s the step-by-step guide with both the commands and the expected output messages from the cluster, making it easier to follow:

### Step 1: Login and Initial Setup

1. **Login as admin**:
   ```bash
   login: admin
   ```
   Expected output:
   ```
   Password:
   ```

2. **Start the cluster setup**:
   ```bash
   ::> cluster setup
   ```
   Expected output:
   ```
   Welcome to the cluster setup wizard.

   You can enter the following commands at any time:
   "help" to display detailed descriptions of any command.
   "exit" to exit the cluster setup wizard.

   Cluster setup will begin now.
   ```

3. **Confirm to continue**:
   ```bash
   Type yes to confirm and continue {yes}: yes
   ```
   Expected output:
   ```
   Enter the node management interface port [e0c]: 
   ```

4. **Configure the node management interface**:
   ```bash
   Enter the node management interface port [e0c]: e0c
   Enter the node management interface IP address [169.254.47.21]: 172.16.0.2
   Enter the node management interface netmask [169.254.47.21]: 255.255.224.0
   Enter the node management interface default gateway: 172.16.0.1
   ```
   Expected output:
   ```
   The node management interface has been configured with the IP address 172.16.0.2 and netmask 255.255.224.0.
   ```

5. **Complete the setup using the command line interface**:
   ```bash
   Press Enter to complete cluster setup using the command line interface.
   ```
   Expected output:
   ```
   Use your web browser to complete cluster setup by accessing https://172.16.0.2

   Otherwise, press Enter to complete cluster setup using the command line interface:
   ```

6. **Create a new cluster**:
   ```bash
   Do you want to create a new cluster or join an existing cluster? {create, join}: create
   ```
   Expected output:
   ```
   Creating a new cluster.
   ```

7. **Indicate that this is not a single-node cluster**:
   ```bash
   Do you intend for this node to be used as a single node cluster? {yes, no} [no]: no
   ```
   Expected output:
   ```
   Existing cluster interface configuration found:

   Port    MTU     IP                Netmask
   e0a     9000    169.254.149.83    255.255.0.0
   e0b     9000    169.254.72.12     255.255.0.0

   Do you want to use this configuration? {yes, no} [yes]: yes
   ```

8. **Use the existing cluster interface configuration**:
   ```bash
   Do you want to use this configuration? {yes, no} [yes]: yes
   ```
   Expected output:
   ```
   Configuration applied.
   ```

9. **Set the cluster administrator password**:
   ```bash
   Enter the cluster administrator's (username "admin") password: cluster1_2024
   Retype the password: cluster1_2024
   ```
   Expected output:
   ```
   Administrator password set successfully.
   ```

### Step 2: Cluster Configuration

10. **Create the cluster**:
    ```bash
    Enter the cluster name: Cluster1
    ```
    Expected output:
    ```
    Creating cluster Cluster1...
    Running final setup tasks...
    ```

11. **Run final setup tasks**:
    Expected output:
    ```
    Cluster Cluster1 has been created.
    ```

12. **Set up a Vserver for Cluster Administration**:
    ```bash
    Enter the cluster management interface port [e0d]: e0d
    Enter the cluster management interface IP address: 172.16.0.3
    Enter the cluster management interface netmask: 255.255.224.0
    Enter the cluster management interface default gateway [172.16.0.1]: 
    ```
    Expected output:
    ```
    A cluster management interface on port e0d with IP address 172.16.0.3 has been created.
    You can use this address to connect to and manage the cluster.
    ```

13. **Skip DNS domain names**:
    ```bash
    Enter the DNS domain names: 
    ```
    Expected output:
    ```
    No DNS domain names were entered.
    ```

14. **Skip Storage Failover (SFO) configuration**:
    ```bash
    SFO will not be enabled on a non-HA system.
    ```
    Expected output:
    ```
    SFO configuration skipped.
    ```

15. **Complete the node setup**:
    ```bash
    Where is the controller located []:
    ```
    Expected output:
    ```
    Cluster "Cluster1" has been created.

    To complete cluster setup, you must join each additional node to the cluster by running "system node show-discovered" and "cluster add-node" from a node in the cluster.

    To complete system configuration, you can use either OnCommand System Manager or the Data ONTAP command-line interface.

    To access OnCommand System Manager, point your web browser to the cluster management IP address (https://172.16.0.3).

    To access the command-line interface, connect to the cluster management IP address (for example, ssh admin@172.16.0.3).
    ```

16. **Check the network interfaces**:
    ```bash
    Cluster1::> net int show
    ```
    Expected output:
    ```
    Vserver    Logical    Status     Network           Current      Current    Is
               Interface  Admin/Oper Address/Mask      Node         Port       Home
    ---------- ---------- ---------- ---------------- ------------ --------- ----
    Cluster    Cluster1-01_clus1 up/up 169.254.149.83/16 Cluster1-01 e0a       true
               Cluster1-01_clus2 up/up 169.254.72.12/16  Cluster1-01 e0b       true
    Cluster1   Cluster1-01_mgmt_auto up/up 172.16.0.2/19 Cluster1-01 e0c      true
               cluster_mgmt up/up 172.16.0.3/19 Cluster1-01 e0d      true

    4 entries were displayed.
    ```

### Step 3: Add Node Two to the Cluster

1. **Login as admin**:
   ```bash
   login: admin
   ```
   Expected output:
   ```
   Password:
   ```

2. **Join the node to the cluster**:
   ```bash
   ::> cluster join -clusteripaddr 169.254.149.83
   ```
   Expected output:
   ```
   Joining cluster

   System checks ..

   Starting cluster support services ..

   This node has joined the cluster Cluster1.
   ```

### Step 4: Configure Diagnostic Privileges and Add Disks

1. **Unlock and set a password for diag user**:
   ```bash
   cluster1::> security login unlock -username diag
   cluster1::> security login password -username diag
   Enter a new password: cluster1_2024
   Enter it again: cluster1_2024
   ```
   Expected output:
   ```
   User "diag" has been unlocked.
   Password changed successfully for user diag.
   ```

2. **Set diagnostic privileges**:
   ```bash
   cluster1::> set -privilege diag
   ```
   Expected output:
   ```
   Warning: These diagnostic commands are for use by NetApp personnel only.
   Do you want to continue? {y|n}: yes
   ```

3. **Access the system shell**:
   ```bash
   cluster1::*> systemshell local
   diag@127.0.0.1's password:
   ```
   Expected output:
   ```
   Warning: The system shell provides access to low-level diagnostic tools that can cause irreparable damage to the system if not used properly. Use this environment only when directed to do so by support personnel.
   ```

4. **Set environment path**:
   ```bash
   cluster1-01% setenv PATH "${PATH}:/usr/sbin"
   ```
   Expected output:
   ```
   Environment path set.
   ```

5. **Navigate to the disk directory**:
   ```bash
   cluster1-01% cd /sim/dev
   ```
   Expected output:
   ```
   Directory changed to /sim/dev.
   ```

6. **Create disks**:
   ```bash
   cluster1-01% sudo vsim_makedisks -n 14 -t 23 -a 2
   ```
   Expected output:
   ```
   Creating ./disks/v2.16:NETAPP ... -VD-1000MB-FZ-520:22814408:2104448
   Creating ./disks/v2.17:NETAPP ... -VD-1000MB-FZ-520:22814408:2104448
   ...
   ```

7. **Create more disks**:
   ```bash
   cluster1-01% sudo vsim_makedisks -n 14 -t 35 -a 3
   ```
   Expected output:
   ```
   Creating ./disks/v3.16:NETAPP ... -VD-500MB-SS-520:25852602:1000448
   Creating ./disks

/v3.17:NETAPP ... -VD-500MB-SS-520:25852602:1000448
   ...
   ```

### Step 5: Increase Volume Size

1. **Return to admin mode**:
   ```bash
   cluster1::> set admin
   ```
   Expected output:
   ```
   Privilege level set to admin.
   ```

2. **Run local commands**:
   ```bash
   cluster1::> run local
   ```
   Expected output:
   ```
   Type 'exit' or 'Ctrl-D' to return to the CLI.
   ```

3. **Delete snapshots and set snapshot schedule**:
   ```bash
   cluster1-01% snap delete -a -f vol0
   ```
   Expected output:
   ```
   Deleted vol0 snapshot hourly.0.
   ```

4. **Set snapshot schedule**:
   ```bash
   cluster1-01% snap sched vol0 0 0 0 0
   ```
   Expected output:
   ```
   Snapshot schedule set for vol0.
   ```

5. **Add disks to the aggregate**:
   ```bash
   cluster1::> aggr add-disks -aggregate aggr0_Cluster1_01 -diskcount 10 -disktype FCAL
   ```
   Expected output:
   ```
   Warning: Aggregate "aggr0_Cluster1_01" is a root aggregate. Adding disks to the root aggregate is not recommended. Once added, disks cannot be removed without reinitializing the node.
   Do you want to continue? {y|n}: y
   ...
   Aggregate capacity available for volume use would be increased by 8.79GB.
   ```

6. **Modify the volume size**:
   ```bash
   Cluster1::> vol modify -vserver Cluster1-01 -volume vol0 -size +8g
   ```
   Expected output:
   ```
   Volume modify successful on volume vol0 of Vserver cluster1-01.
   ```

7. **Check the volume and aggregate status**:
   ```bash
   Cluster1::> vol show
   ```
   Expected output:
   ```
   Vserver    Volume   Aggregate         State   Type   Size      Available   Used%
   -------    -------  ---------         -----   ----   ----      ---------   -----
   Cluster1-01 vol0    aggr0_Cluster1_01 online  RW     8GB       7.26GB      4%
   Cluster1-02 vol0    aggr0_Cluster1_02 online  RW     8GB       7.26GB      4%
   ```

This guide includes both the commands to enter and the corresponding messages you should expect from the cluster. This structure should make it easier to follow and execute the steps accurately.
