Here is the condensed process and commands for powering up a NAS, logging into the cluster, and performing the necessary health checks and configurations:

### Power Up and Login Process

1. **Boot ONTAP from LOADER-B (if encountered):**
    ```bash
    LOADER-B> boot_ontap 
    or
    LOADER-B> autoboot
    ```

2. **Login via SP or BMC:**
    ```bash
    ssh -l admin 19.116.0.140
    BMC ltp80103> system console
    Use ESC+t to exit
    login: $ooluwada
    Password: same as BMC password
    ```

### Cluster Shell Commands

1. **Run the following commands once logged in:**
    ```bash
    system node autosupport invoke -node * -type all -message MAINT=end
    cluster ha modify –configured true
    storage failover show -enabled false
    storage failover show -possible false
    storage failover modify –node node1 –enabled true
    storage failover modify –node node2 –enabled true
    ```

2. **Check for config-lock and release if necessary:**
    ```bash
    vserver show -fields config-lock
    set diag
    vserver unlock -vserver vserver1 -force true
    vserver start -vserver vserver1
    ```

3. **Verify LIFs and revert if necessary:**
    ```bash
    net int show -is-home false
    network interface revert -lif vserver1-esx01
    net int revert *
    ```

4. **Ensure all LIFs are running:**
    ```bash
    vol show -fields comment
    net int show
    ```

### Health Checks and Final Configuration

1. **Run the following checks:**
    ```bash
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
    ```

2. **Trigger AutoSupport:**
    ```bash
    system node autosupport invoke -node * -type all -message MAINT=end
    ```

### Start of Script

```bash
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
system controller service-event show
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
network port reachability show -reachability-status !ok -expected-broadcast-domain !"-" 
network port show -health-status degraded
network port show -link !up -up-admin true

### DATA ###
lun show -state !online
snapmirror show -healthy false
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
```

### End of Script

This streamlined process should help you quickly power up and check the health of your NAS system without too much explanation clutter.