
```bash
login: admin
::> cluster setup
Type yes to confirm and continue {yes}: yes
Enter the node management interface port [e0c]: e0c
Enter the node management interface IP address [169.254.47.21]: 172.16.0.2
Enter the node management interface netmask [169.254.47.21]: 255.255.224.0
Enter the node management interface default gateway: 172.16.0.1
```

```bash

Use your web browser to complete cluster setup by accessing https://172.16.0.2

Otherwise, press Enter to complete cluster setup using the command line interface:

Do you want to create a new cluster or join an existing cluster? {create, join}: create

Do you intend for this node to be used as a single node cluster? {yes, no} [no]: no

Existing cluster interface configuration found:

Port    MTU     IP                Netmask
e0a     9000    169.254.149.83    255.255.0.0
e0b     9000    169.254.72.12     255.255.0.0

Do you want to use this configuration? {yes, no} [yes]: yes

Enter the cluster administrator's (username "admin") password: cluster1_2024

Retype the password: cluster1_2024

Do you want to use this configuration? {yes, no} [yes]: yes

Enter the cluster administrator's (username "admin") password:

Retype the password:

Step 1 of 5: Create a Cluster
You can type "back", "exit", or "help" at any question.

Enter the cluster name: Cluster1
Creating cluster Cluster1

Running final setup tasks

Cluster Cluster1 has been created.

Step 2 of 5: Add Feature License Keys
You can type "back", "exit", or "help" at any question.

Enter an additional license key []:

Step 3 of 5: Set Up a Vserver for Cluster Administration
You can type "back", "exit", or "help" at any question.

Enter the cluster management interface port [e0d]:
Enter the cluster management interface IP address: 172.16.0.3
Enter the cluster management interface netmask: 255.255.224.0
Enter the cluster management interface default gateway [172.16.0.1]:

A cluster management interface on port e0d with IP address 172.16.0.3 has been created. You can use this address to connect to and manage the cluster.

Enter the DNS domain names:

Step 4 of 5: Configure Storage Failover (SFO)
You can type "back", "exit", or "help" at any question.

SFO will not be enabled on a non-HA system.

Step 5 of 5: Set Up the Node
You can type "back", "exit", or "help" at any question.

Where is the controller located []:

Cluster "Cluster1" has been created.

To complete cluster setup, you must join each additional node to the cluster by running "system node show-discovered" and "cluster add-node" from a node in the cluster.

To complete system configuration, you can use either OnCommand System Manager or the Data ONTAP command-line interface.

To access OnCommand System Manager, point your web browser to the cluster management IP address (https://172.16.0.3).

To access the command-line interface, connect to the cluster management IP address (for example, ssh admin@172.16.0.3).

Cluster1::> net int show
(network interface show)
Vserver    Logical    Status     Network           Current      Current    Is
           Interface  Admin/Oper Address/Mask      Node         Port       Home
---------- ---------- ---------- ---------------- ------------ --------- ----
Cluster    Cluster1-01_clus1 up/up 169.254.149.83/16 Cluster1-01 e0a       true
           Cluster1-01_clus2 up/up 169.254.72.12/16  Cluster1-01 e0b       true

Cluster1   Cluster1-01_mgmt_auto up/up 172.16.0.2/19 Cluster1-01 e0c      true
           cluster_mgmt up/up 172.16.0.3/19 Cluster1-01 e0d      true

4 entries were displayed.
Cluster1::>
```

Now add node two to the cluster
```bash
System initialization has completed successfully.
ERROR: missing pmroot_late.tgz file
wrote key file "/tmp/rndc.key"
The VMware service must be run from within a virtual machine.
Log: The VMware service must be run from within a virtual machine.
Log: Backtrace:
******************************************
UNABLE TO FETCH NODE MANAGEMENT IP.
PLEASE REFER TO DOCUMENTATION FOR MORE DETAILS.
******************************************
FOR CONFIGURING A 2-NODE CLUSTER PLEASE REFER TO THE DOCUMENTATION
BEFORE PROCEEDING WITH CLUSTER SETUP
******************************************

Tue Aug  6 17:43:55 UTC 2024
login: admin
::> cluster join -clusteripaddr 169.254.149.83
Joining cluster

System checks ..

Starting cluster support services ..

This node has joined the cluster Cluster1.
Cluster1::>

```

For each node set diag privilage loggin to the root to add disk, then back to the node shell to add disk to the aggregate and increase vol0 size
```bash
cluster1::> security login unlock -username diag
cluster1::> security login password -username diag

Enter a new password: cluster1_2024
Enter it again: cluster1_2024



cluster1::> set -privilege diag
Warning: These diagnostic commands are for use by NetApp personnel only.
Do you want to continue? {y|n}: yes

```

``` bash
cluster1::*> systemshell local
(system node systemshell)
diag@127.0.0.1's password:

Warning: The system shell provides access to low-level diagnostic tools that can cause irreparable damage to the system if not used properly. Use this environment only when directed to do so by support personnel.

```

```bash

cluster1-01% setenv PATH "${PATH}:/usr/sbin"

cluster1-01% cd /sim/dev

cluster1-01% sudo vsim_makedisks -n 14 -t 23 -a 2
Creating ./disks/v2.16:NETAPP ... -VD-1000MB-FZ-520:22814408:2104448
Creating ./disks/v2.17:NETAPP ... -VD-1000MB-FZ-520:22814408:2104448
Creating ./disks/v2.18:NETAPP ... -VD-1000MB-FZ-520:22814408:2104448
Creating .>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
Shelf file Shelf:DiskShelf14 updated

cluster1-01% sudo vsim_makedisks -n 14 -t 35 -a 3
Creating ./disks/v3.16:NETAPP ... -VD-500MB-SS-520:25852602:1000448
Creating ./disks/v3.17:NETAPP ... -VD-500MB-SS-520:25852602:1000448
Creating ./disks/v3.18:NETAPP ... -VD-500MB-SS-520:25852602:1000448
Creating .>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
Shelf file Shelf:DiskShelf14 updated
```

``` bash
cluster1::> set admin

cluster1::> run local
Type 'exit' or 'Ctrl-D' to return to the CLI
cluster1-01% snap delete -a -f vol0
Deleted vol0 snapshot hourly.0.
cluster1-01% snap sched vol0 0 0 0 0
cluster1-01%
```

```bash

Cluster1::> aggr add-disks -aggregate aggr0_Cluster1_01 -diskcount 10 -disktype FCAL

Warning: Aggregate "aggr0_Cluster1_01" is a root aggregate. Adding disks to the
root aggregate is not recommended. Once added, disks cannot be removed without reinitializing the node.
Do you want to continue? {y|n}:

Info: Disks would be added to aggregate "aggr0_Cluster1_01" on node "Cluster1-01" in the following manner:

First Plex
RAID Group rg0, 13 disks (block checksum, raid_dp)

Position   Disk       Type       Usable Physical
----------- ---------- ------     -------- --------
data        NET-1.13     FCAL      1000MB   1.00GB
data        NET-1.14     FCAL      1000MB   1.00GB
data        NET-1.15     FCAL      1000MB   1.00GB
data        NET-1.19     FCAL      1000MB   1.00GB
data        NET-1.22     FCAL      1000MB   1.00GB
data        NET-1.23     FCAL      1000MB   1.00GB
data        NET-1.24     FCAL      1000MB   1.00GB
data        NET-1.25     FCAL      1000MB   1.00GB
data        NET-1.26     FCAL      1000MB   1.00GB
data        NET-1.27     FCAL      1000MB   1.00GB

Aggregate capacity available for volume use would be increased by 8.79GB.
Do you want to continue? {y|n}: 
Aggregate        Size   Available   Used%  State  #Vols  Nodes          RAID Status
---------        ----   ---------   -----  -----  -----  -----          -----------
aggr0_Cluster1_01  9.18GB   8.39GB     9%   online 1      Cluster1-01    raid_dp, normal
aggr0_Cluster1_02  9.18GB   8.39GB     9%   online 1      Cluster1-02    raid_dp, normal

2 entries were displayed.

```

``` bash

cluster1::> vol modify -vserver Cluster1-01 -volume vol0 -size +8g
Volume modify successful on volume vol0 of Vserver cluster1-01.
Cluster1::> vol show
Vserver    Volume   Aggregate         State   Type   Size      Available   Used%
-------    -------  ---------         -----   ----   ----      ---------   -----
Cluster1-01 vol0    aggr0_Cluster1_01 online  RW     8GB       7.26GB      4%
Cluster1-02 vol0    aggr0_Cluster1_02 online  RW     8GB       7.26GB      4%

2 entries were displayed.

Cluster1::>


```