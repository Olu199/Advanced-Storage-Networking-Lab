Here’s the streamlined process for shutting down a NAS (CDOT) system and then powering it off:

### Pre-Shutdown Preparation

1. **Confirm All Hosts Are Shutdown:**
   - Ensure that Unix, ESX, Windows servers, and any other hosts accessing the NAS have been properly shut down.
   - Use the bridge or incident ticket to coordinate and confirm this step.

### NAS – Shutdown Procedure

1. **Login to Node 1 via SP:**
   ```bash
   ssh -l admin node01rm
   BMC node01> system console
   ```

   - If prompted:
   ```bash
   SP-login: admin
   Password: (Enter SP password)
   ```

2. **Login to Node 2 via SP:**
   ```bash
   ssh -l admin cap80102rm
   BMC cap80102> system console
   ```

   - If prompted:
   ```bash
   SP-login: admin
   Password: (Enter SP password)
   ```

### Trigger AutoSupport and Shutdown Nodes

1. **Trigger AutoSupport:**
   ```bash
   cap801::> system node autosupport invoke -node * -type all -message "Planned_Shutdown"
   ```

2. **Modify Cluster HA and Storage Failover Settings:**
   ```bash
   cap801::> cluster ha modify -configured false
   cap801::> storage failover modify –node node01 -enabled false
   cap801::> storage failover modify –node cap80102 -enabled false
   ```

3. **Halt the Nodes:**
   ```bash
   cap801::> halt -node node01 -inhibit-takeover true 
   cap801::> halt -node cap80102 -inhibit-takeover true 
   ```

   - **If an error occurs when halting node `cap80102`, run:**
   ```bash
   cap801::> halt -node cap80102 -inhibit-takeover true -ignore-quorum-warnings true
   ```

### Power Off the Nodes via BMC/SP

1. **Power Off Node 1:**
   ```bash
   BMC node01> system power off
   ```

2. **Power Off Node 2:**
   ```bash
   BMC cap80102> system power off
   ```

### Notes:
- Make sure to execute each command carefully and verify that each node is properly halted before powering off.
- This procedure is essential for ensuring that the NAS system is safely shut down, avoiding potential data corruption or other issues during the power-off process.