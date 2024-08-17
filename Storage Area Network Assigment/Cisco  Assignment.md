**Acronym**: L.E.D.E.Z.I.A.S.V.E

**Mnemonic**: **L**arry's **E**xperimental **D**isk **E**xploded, **Z**any **I**nvestigators **A**ssumed **S**quirrels **V**andalized **E**verything.

Story: Larry, ever the tech tinkerer, built an experimental disk drive he believed would revolutionize storage. But one day, it exploded in a puff of smoke! Instead of a technical investigation, the company brought in some zany investigators who had a peculiar fixation on squirrels. They concluded that tech-savvy squirrels had vandalized the equipment, causing the explosion. The tech community had a good laugh, and "squirrel-proofing" became the new buzzword in storage tech forums.

1. **L**ogin to the Switch
2. **E**nter Configuration Mode
3. **D**efine Aliases
4. **E**stablish Zones
5. **Z**one Database Incorporation
6. **I**ntegrate Zones
7. **A**ctivate Zone Configuration
8. **S**ave Configuration
9. **V**erify Zoning
10. **E**xit
---

#### Prerequisites:
- Ensure you have the necessary privileges to perform zoning on the switch.
- Ensure you have connectivity to the switch either through SSH or console.
- The **VSAN ID** in this assignment is made up. In a real word setting, it might not be the same.
Given the information provided, let's draft a detailed work plan for establishing storage zones on Cisco switches connecting the host CHGSAN01WP and Storage CHGIBM01PS.

### Cisco Zoning Assignment:

#### Prerequisites:
- Ensure you have the necessary privileges to perform zoning on the switch.
- Ensure you have connectivity to the switch either through SSH or console.

#### Work Plan:

1. **Login to the Switch**:
   ```bash
   ssh [username]@[switch_ip_address]
   ```

2. **Enter Configuration Mode**:
   ```bash
   conf t
   ```

3. **Define Aliases for the Host and Storage**:
   - For the host:
     ```bash
     fcalias name CHGSAN01WP_HBA_1 vsan [VSAN_ID] member pwwn 50:06:01:60:C8:A0:05:46
     fcalias name CHGSAN01WP_HBA_2 vsan [VSAN_ID] member pwwn 50:06:01:60:C8:A0:05:46
     ```

   - For the storage:
     ```bash
     fcalias name CHGIBM01PS_Node1_Port0 vsan [VSAN_ID] member pwwn 50:06:01:6C:48:A0:02:8F
     fcalias name CHGIBM01PS_Node1_Port1 vsan [VSAN_ID] member pwwn 50:06:01:6D:48:A0:02:8F
     fcalias name CHGIBM01PS_Node2_Port0 vsan [VSAN_ID] member pwwn 50:06:01:64:48:A0:02:8F
     fcalias name CHGIBM01PS_Node2_Port1 vsan [VSAN_ID] member pwwn 50:06:01:65:48:A0:02:8F
     ```

4. **Establish Zones**:
   ```bash
   zone name CHGSAN01WP_to_CHGIBM01PS_Node1_Port0 vsan [VSAN_ID]
   member fcalias CHGSAN01WP_HBA_1
   member fcalias CHGIBM01PS_Node1_Port0

   zone name CHGSAN01WP_to_CHGIBM01PS_Node1_Port1 vsan [VSAN_ID]
   member fcalias CHGSAN01WP_HBA_2
   member fcalias CHGIBM01PS_Node1_Port1

   zone name CHGSAN01WP_to_CHGIBM01PS_Node2_Port0 vsan [VSAN_ID]
   member fcalias CHGSAN01WP_HBA_1
   member fcalias CHGIBM01PS_Node2_Port0

   zone name CHGSAN01WP_to_CHGIBM01PS_Node2_Port1 vsan [VSAN_ID]
   member fcalias CHGSAN01WP_HBA_2
   member fcalias CHGIBM01PS_Node2_Port1
   ```

5. **Incorporate Zones into Zone Database**:
   - For Fabric A:
     ```bash
     zoneset name CFG_FAB_A vsan [VSAN_ID]
     member CHGSAN01WP_to_CHGIBM01PS_Node1_Port0
     member CHGSAN01WP_to_CHGIBM01PS_Node1_Port1
     member CHGSAN01WP_to_CHGIBM01PS_Node2_Port0
     member CHGSAN01WP_to_CHGIBM01PS_Node2_Port1
     ```

   - For Fabric B (Repeat the same steps but with CFG_FAB_B).

6. **Activate the Zone Configuration**:
   ```bash
   zoneset activate name CFG_FAB_A vsan [VSAN_ID]
   ```

   For Fabric B:
   ```bash
   zoneset activate name CFG_FAB_B vsan [VSAN_ID]
   ```

7. **Save the Configuration**:
   ```bash
   copy running-config startup-config
   ```

8. **Verify the Zoning Configuration**:
   ```bash
   show zone active vsan [VSAN_ID]
   ```

9. **Exit Configuration Mode**:
   ```bash
   end
   ```

10. **Logout from the Switch**:
    ```bash
    exit
    ```

**Note**: Replace `[VSAN_ID]` with the appropriate VSAN ID for your environment. Ensure you have backups of your switch configurations before making any changes. Always follow best practices and guidelines specific to your environment and organization when making configuration changes.

**Note**: Replace `[VSAN_ID]` with the appropriate VSAN ID for your environment. Ensure you have backups of your switch configurations before making any changes. Always follow best practices and guidelines specific to your environment and organization when making configuration changes.