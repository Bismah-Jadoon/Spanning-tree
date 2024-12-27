### README: VLAN Configuration using Spanning Tree Protocol

#### Overview
This project implements VLAN segmentation in a network using **Spanning Tree Protocol (STP)** with Rapid PVST+ to optimize network performance and prevent loops. Below is a detailed description of the setup and steps taken to configure the network.

---

#### Objectives
1. Configure VLANs and assign devices to respective VLANs.
2. Implement **Rapid PVST+** as the spanning-tree protocol.
3. Set up **PortFast** for faster connectivity on edge ports.
4. Configure trunk ports to allow VLAN tagging across switches.
5. Designate root bridges for specific VLANs to ensure STP stability.
6. Manually add VLANs to all switches as per the topology.

---

#### Steps for Configuration

1. **Hostname and Spanning Tree Configuration**
   - Each switch is assigned a unique hostname for identification.
   - Spanning-tree mode is set to **Rapid PVST+**.
   ```plaintext
   S1(config)#hostname S1
   S1(config)#spanning-tree mode rapid-pvst
   ```

2. **Access Ports and PortFast**
   - Access ports are configured for specific VLANs:
     - **VLAN 10**: FastEthernet 0/1–0/8
     - **VLAN 20**: FastEthernet 0/9–0/16
   - **PortFast** is enabled to bypass the STP listening and learning states on access ports.
   ```plaintext
   S1(config)#interface range FastEthernet 0/1-8
   S1(config-if-range)#switchport mode access
   S1(config-if-range)#switchport access vlan 10
   S1(config-if-range)#spanning-tree portfast

   S1(config)#interface range FastEthernet 0/9-16
   S1(config-if-range)#switchport mode access
   S1(config-if-range)#switchport access vlan 20
   S1(config-if-range)#spanning-tree portfast
   ```

3. **Trunk Ports**
   - Trunk ports are configured to allow VLAN communication between switches.
     - Trunk: **GigabitEthernet 0/1–0/2** and FastEthernet 0/24.
   ```plaintext
   S1(config)#interface range GigabitEthernet 0/1-2
   S1(config-if-range)#switchport mode trunk

   S1(config)#interface FastEthernet 0/24
   S1(config-if-range)#switchport mode trunk
   ```

4. **Root Bridge Configuration**
   - Specific switches are designated as the **Root Bridge** for each VLAN:
     - **VLAN 10**: Switch S1
     - **VLAN 20**: Switch S3
     - **VLAN 30**: Switch S5
   ```plaintext
   S1(config)#spanning-tree vlan 10 root primary
   S3(config)#spanning-tree vlan 20 root primary
   S5(config)#spanning-tree vlan 30 root primary
   ```

5. **PC IP Address Configuration**
   - PCs are assigned static IP addresses based on their VLAN subnet. Example:
     - **VLAN 10 (Human Resources)**: `192.168.10.0/24`
     - **VLAN 20 (Finance)**: `192.168.20.0/24`
     - **VLAN 30 (Legal)**: `192.168.30.0/24`
   - Configuration Path: **Desktop > IP Configuration** on each PC.

6. **Manual VLAN Creation**
   - VLANs are manually added to each switch to ensure consistency across the network.
   ```plaintext
   S1(config)#vlan 30
   S2(config)#vlan 20
   S4(config)#vlan 10
   S5(config)#vlan 10
   S5(config)#vlan 20
   S5(config)#vlan 30
   S6(config)#vlan 10
   S6(config)#vlan 20
   ```

7. **Save Configuration**
   - The final topology is saved as `STP exercise without VTP.pkt`.

---

#### VLAN-to-Switch Mapping
- **VLAN 10 (192.168.10.0/24)**:
  - Switches: S1, S4, S5, S6
- **VLAN 20 (192.168.20.0/24)**:
  - Switches: S1, S3, S5, S6
- **VLAN 30 (192.168.30.0/24)**:
  - Switches: S5

---

#### Key Features
1. **Rapid PVST+:** Ensures fast convergence and loop-free topology.
2. **PortFast:** Reduces downtime for PCs connected to access ports.
3. **Manual VLAN Management:** VLANs created explicitly without VTP dependency.

---

This configuration ensures a scalable, loop-free network with optimized VLAN segmentation for better security and performance.
