**L.E.C.C.A.S.E.V.**

**L**ucy **E**njoys **C**ooking **C**hocolate **A**pple **S**trudel **E**very **V**alentine.

Here's the breakdown:

- **L**ucy: **L**ogin to the Switch
- **E**njoys: **E**nter Configuration Mode
- **C**ooking: **C**reate Aliases for Devices
- **C**hocolate: **C**reate Zones and Add Aliases to Zone
- **A**pple: **A**dd Zones to Configuration
- **S**trudel: **S**ave Configuration
- **E**very: **E**nable the Configuration
- **V**alentine: **V**erify the Zoning
---
### Brocade Zoning for CHGSAN01WP and CHGIBM01PS

1. **Login to the Switch**:
   - Use SSH or a terminal emulator to connect to the switch.
   - Login using your credentials.

2. **Enter Configuration Mode**:
   ```bash
   cfgshow
   ```

3. **Create Aliases for Devices**:
   - For the server:
   ```bash
   aliCreate "CHGSAN01WP_HBA1", "50:06:01:60:C8:A0:05:46"
   aliCreate "CHGSAN01WP_HBA2", "50:06:01:60:C8:A0:05:46"
   ```
   - For the storage:
   ```bash
   aliCreate "CHGIBM01PS_Node1_Port0", "50:06:01:6C:48:A0:02:8F"
   aliCreate "CHGIBM01PS_Node1_Port1", "50:06:01:6D:48:A0:02:8F"
   aliCreate "CHGIBM01PS_Node2_Port0", "50:06:01:64:48:A0:02:8F"
   aliCreate "CHGIBM01PS_Node2_Port1", "50:06:01:65:48:A0:02:8F"
   ```

4. **Create Zones and Add Aliases to Zone** :
   - For Fabric A:
   ```bash
   zoneCreate "Zone_CHGSAN01WP_to_CHGIBM01PS_FabricA", "CHGSAN01WP_HBA1; CHGIBM01PS_Node1_Port0; CHGIBM01PS_Node1_Port1"
   ```
   - For Fabric B:
   ```bash
   zoneCreate "Zone_CHGSAN01WP_to_CHGIBM01PS_FabricB", "CHGSAN01WP_HBA2; CHGIBM01PS_Node2_Port0; CHGIBM01PS_Node2_Port1"
   ```

5. **Add Zones to Configuration**:
   - For Fabric A:
   ```bash
   cfgAdd "CFG_FAB_A", "Zone_CHGSAN01WP_to_CHGIBM01PS_FabricA"
   ```
   - For Fabric B:
   ```bash
   cfgAdd "CFG_FAB_B", "Zone_CHGSAN01WP_to_CHGIBM01PS_FabricB"
   ```

6. **Save Configuration**:
   ```bash
   cfgSave
   ```

7. **Enable the Configuration**:
   - For Fabric A:
   ```bash
   cfgEnable "CFG_FAB_A"
   ```
   - For Fabric B:
   ```bash
   cfgEnable "CFG_FAB_B"
   ```

8. **Verify the Zoning**:
   ```bash
   zoneshow
   ```

This is a basic zoning procedure for Brocade switches. Make sure to always backup your switch configuration before making any changes. Also, it's a good practice to verify the zoning configuration with the storage and server teams before enabling it.