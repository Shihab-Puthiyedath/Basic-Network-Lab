# Enterprise Multi-VLAN Inter-VLAN Routing & Centralized DHCP Relay Lab

![Network Topology](./topology.png)

## 1. Network Topology Overview & Description
This repository features a complete, production-grade enterprise network simulation built in Cisco Packet Tracer. The design implements **Router-on-a-Stick (Inter-VLAN Routing)** across a core layer, dual distribution/access switches, and segmented departmental zones. It incorporates centralized services where end-devices across multiple broadcast domains pull dynamic IP configurations via DHCP relay (`ip helper-address`). Additionally, port security with sticky MAC address learning and rapid spanning-tree features are implemented for enhanced edge security.

---

## 2. VLAN & Subnet Allocation Matrix

| VLAN ID | Name | Subnet / Mask | Gateway IP | Description / Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | MGMNT | `192.168.10.0/24` | `192.168.10.1` | Management Network for ASW1 |
| **VLAN 20** | ACCOUNTS | `192.168.20.0/24` | `192.168.20.1` | Accounts Department (PC1) |
| **VLAN 30** | IT | `192.168.30.0/24` | `192.168.30.1` | IT Department (PC2) |
| **VLAN 40** | OPERATIONS | `192.168.40.0/24` | `192.168.40.1` | Operations Department (PC3) |
| **VLAN 50** | SERVERS | `192.168.50.0/24` | `192.168.50.1` | Central Services & DHCP Server (`SRV1`)[cite: 2] |
| **VLAN 60** | MGMNT | `192.168.60.0/24` | `192.168.60.1` | Management Network for ASW-2 |

---

## 3. Core Device Configurations Summary

### Core Router (`CORE-RTR`)
* Configured with 802.1Q subinterfaces across `GigabitEthernet0/1` and `GigabitEthernet0/2`.
* Utilizes `ip helper-address 192.168.50.11` to relay dynamic broadcast requests from client VLANs to the centralized server.

### Access Switch 1 (`ASW1`)
* Manages **VLAN 10** (Management IP: `192.168.10.11`), **VLAN 20**, and **VLAN 30**.
* Features port security (sticky MAC learning) and PortFast edge on user access ports (`FastEthernet0/1` and `FastEthernet0/2`).
* Trunk link configured on `GigabitEthernet0/1` allowing VLANs `10,20,30`.

### Access Switch 2 (`ASW-2`)
* Manages **VLAN 40**, **VLAN 50** (Server/SRV1 access), and **VLAN 60** (Management IP: `192.168.60.11`).
* Features port security and PortFast on edge ports (`FastEthernet0/1` and `FastEthernet0/2`).
* Trunk link configured on `GigabitEthernet0/1` allowing VLANs `40,50,60`.

---

## 4. Lab Practice & Verification Steps

After importing the `.pkt` file into Cisco Packet Tracer, perform these steps to verify full functionality:

1. **Verify DHCP IP Allocation:** On client PCs, run the following commands in the command prompt to request a fresh lease[cite: 2]:
   ```cmd
   ipconfig /release
   ipconfig /renew
