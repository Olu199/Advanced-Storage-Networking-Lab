_____________________________
NAS – Power up
once the device has booted, the best way to login is true the sp or bmc
below are the process to login into the cluster



if LOADER-B encounter
LOADER-B> boot_ontap 
or
LOADER-B> autoboot

ooluwada@ito079596:~> ssh -l admin 19.116.0.140
password: same as 
BMC ltp80103> system console
Use ESC+t to exit
press enter or any random
login: $ooluwada
Password: same as 


Once they done the physical power up, we could able to login the cluster directly. 

Once logged in, run the below commands from the cluster shell.


system node autosupport invoke -node * -type all -message MAINT=end
node1_cluster1::> cluster ha modify –configured true
node1_cluster1::> storage failover show -enabled false
node1_cluster1::> storage failover show -possible false
node1_cluster1::> storage failover modify –node node1 –enabled true
node1_cluster1::> storage failover modify –node node2 –enabled true

Please verify the vservers have config-lock. If yes, release them and start the vservers.

node1_cluster1::> vserver show -fields config-lock
vserver config-lock
------- -----------
node1_cluster1 false
vserver1 true
vserver2 false
node1_cluster101 -
node1_cluster102 -
5 entries were displayed.

Switch to diag mode and unlock the vserver.

node1_cluster1::> set diag

Warning: These diagnostic commands are for use by NetApp personnel only.
Do you want to continue? {y|n}: y

node1_cluster1::*> vserver unlock -vserver vserver1 -force true

node1_cluster1::*> vserver start -vserver vserver1
[Job 7193] Job succeeded: DONE

node1_cluster1::*> vserver show

node1_cluster1::*> vserver show

Please check that all LIFs are in their home node. 

node1_cluster1::> net int show -is-home false


If any LIFs are existing in their partner node, revert those LIFs to their home node once verified the port status of both nodes.
node1_cluster1::*> network interface revert -lif vserver1-esx01
node1_cluster1::*> ifgrp show

node1_cluster1::> net int revert *

Once reverted all the LIFs, please do check the ping response against those IPs.

Finally, please ensure that Admin/Operational state would be running/running and up/up for vservers & LIFs.



node1_cluster1::> vol show -fields comment
                               Admin      Operational Root
Vserver     Type    Subtype    State      State       Volume     Aggregate
----------- ------- ---------- ---------- ----------- ---------- ----------
node1_cluster1      admin   -          -          -           -          -
vserver1 data    default    running    running     cap8101bu0 node1_cluster101_
                                                      1_root     sas_aggr1
vserver2 data    default    running    running     cap8101nn0 node1_cluster102_
                                                      1_root     sata_aggr1
node1_cluster101    node    -          -          -           -          -
node1_cluster102    node    -          -          -           -          -
5 entries were displayed.

node1_cluster1::> net int show
  (network interface show)
            Logical    Status     Network            Current       Current Is
Vserver     Interface  Admin/Oper Address/Mask       Node          Port    Home
----------- ---------- ---------- ------------------ ------------- ------- ----
Cluster
            node1_cluster101_clus1 up/up  169.254.32.144/16  node1_cluster101      e0a     true
            node1_cluster101_clus2 up/up  169.254.156.20/16  node1_cluster101      e0b     true
            node1_cluster102_clus1 up/up  169.254.88.80/16   node1_cluster102      e0a     true
            node1_cluster102_clus2 up/up  169.254.228.115/16 node1_cluster102      e0b     true
node1_cluster1
            node1_cluster101_mgmt1 up/up  19.126.0.121/26    node1_cluster101      a0a-501 true
            node1_cluster101_vsm1 up/up   19.126.0.122/26    node1_cluster101      a0a-501 true
            node1_cluster102_mgmt1 up/up  19.126.0.123/26    node1_cluster102      a0a-501 true
            node1_cluster102_vsm1 up/up   19.126.0.124/26    node1_cluster102      a0a-501 true
            cluster_mgmt up/up    19.126.0.120/26    node1_cluster102      a0a-501 true
vserver1
            vserver1-esx01 up/up 19.126.111.230/28 node1_cluster102     a0a-995 true
            vserver1-esx02 up/up 19.126.111.237/28 node1_cluster102     a0a-995 true
            vserver1-mgmt01 up/up 19.126.0.125/26 node1_cluster102      a0a-501 true
vserver2
            vserver2-mgmt01 up/up 19.126.0.126/26 node1_cluster102      a0a-501 true
13 entries were displayed.

Please do complete the remaining standard health checks and update in the bridge.

vol show -fields comment

node show
storage failover show
storage aggregate show -state !online
volume show -state !online
volume show -is-inconsistent true
network interface show -role data -failover
vserver show -type data -operational-state !running
system health subsystem show
node run -node * -command storage show fault

Trigger autosuppport:
node1_cluster1::> system node autosupport invoke -node * -type all -message MAINT=end


### START OF SCRIPT ###
set -rows 0
network interface check cluster-connectivity start -node *

### GENERAL ###
cluster kernel-service show -status-quorum out-of-quorum
cluster show -health false
job show -state Failure|Error|Quit|Dead|Unknown
system chassis show -status !ok
system controller config show-errors -description !"sysconfig: There are no configuration errors."*
system controller environment show -status !ok
system controller memory dimm show -status !ok
system controller service-event show #Requires current supported ONTAP platforms
system health alert show
system health subsystem show -health !ok
system node environment sensors show -state !normal
system service-processor show -status !online
system services web node show -status !online

### STORAGE ###
storage aggregate show -state !online
storage aggregate show -raidstatus !*normal*
storage aggregate object-store show -object-store-availability unavailable 
storage bridge show -status !ok
storage disk show -state !spare,!present
storage errors show
storage failover show -enabled false
storage failover show -partner-aggregates !false -state-description !"Non-HA mode"
storage failover show -possible false
storage port show -errors
storage shelf show -op-status !Normal

### NETWORK ###
network fcp adapter show -is-conn-established false -status-admin up
network interface show -is-home false
network interface show -status-oper down -status-extended !"This LIF is down because its Vserver is stopped"*
network port ifgrp show -mode !singlemode -activeports !full
network port ifgrp show -mode singlemode -activeports !partial
network port reachability show -reachability-status !ok -expected-broadcast-domain !"-" #Requires ONTAP 9.8+
network port show -health-status degraded
network port show -link !up -up-admin true

### DATA ###
lun show -state !online
snapmirror show -healthy falseS
volume show -in-nvfailed-state true
volume show -is-incSonsistent true
volume show -state !online,!restricted
vserver cifs check -status down 
vserver cifs show -status-admin down
vserver fcp show -status-admin down
vserver iscsi show -status-admin down
vserver nfs show -access !true
vserver show -type data -operational-state !running -subtype !dp-destination,!sync-destination
network interface check cluster-connectivity show -node * -loss !None -date-time >10m 
set -privilege admin

### END OF SCRIPT ###
