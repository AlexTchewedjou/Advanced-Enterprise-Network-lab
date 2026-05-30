# 🏢 Enterprise Campus Network 

A full-featured enterprise network simulation built in NetSim, demonstrating core CCNA skills across VLANs, OSPF routing, STP redundancy, ACL security, NAT, and DHCP.

---

## 📌 Project Overview

This project simulates a **dual-site enterprise network** — a headquarters (HQ) and a branch office — connected via OSPF over a WAN link. It was designed as a capstone project for the Cisco CCNA certification and covers every major exam domain in a realistic, production-style topology.

**Simulator:** NetSim (Boson)    
**Topology:** Multi-layer campus with redundant access layer and OSPF inter-site routing

---

## 🗺️ Network Topology

---
                        [ISP / Cloud Simulation]
                                  |
                          [Branch-RTR]
                         /             \
               (OSPF WAN Link)       [Branch-SW]
               192.168.100.0/30       |
                        |           VLAN 10, 20
                   [HQ-RTR]        (Branch LAN)
                        |
               [Core-MLS] ← Layer 3 Switch / Inter-VLAN Routing
               /                        \
        [Access-SW1]               [Access-SW2]
         VLAN 10 — HR               VLAN 30 — Servers
         VLAN 20 — IT               VLAN 40 — Management
---

---

## 📋 IP Addressing Scheme

| Device        | Interface         | IP Address         | Description         |
|---------------|-------------------|--------------------|---------------------|
| HQ-RTR        | G0/0              | 10.0.0.1/30        | Link to Core-MLS    |
| HQ-RTR        | G0/1              | 192.168.100.1/30   | WAN to Branch       |
| Branch-RTR    | G0/0              | 192.168.100.2/30   | WAN to HQ           |
| Branch-RTR    | G0/1              | 172.16.0.1/24      | Branch LAN          |
| Core-MLS      | VLAN 10 SVI       | 10.10.10.1/24      | HR Gateway          |
| Core-MLS      | VLAN 20 SVI       | 10.20.20.1/24      | IT Gateway          |
| Core-MLS      | VLAN 30 SVI       | 10.30.30.1/24      | Servers Gateway     |
| Core-MLS      | VLAN 40 SVI       | 10.40.40.1/24      | Management Gateway  |
| Core-MLS      | G0/1              | 10.0.0.2/30        | Uplink to HQ-RTR    |

---

## ✅ Skills Demonstrated

### 1. VLANs & Inter-VLAN Routing
- 4 VLANs configured across HQ access layer (HR, IT, Servers, Management)
- 802.1Q trunking between all switches
- Layer 3 SVIs on Core-MLS for inter-VLAN routing
- IP routing enabled on multilayer switch

### 2. OSPF (Single Area)
- OSPF Area 0 across HQ-RTR, Core-MLS, and Branch-RTR
- Unique router IDs assigned per device
- Passive interfaces on all end-user-facing SVIs and LAN ports
- OSPF verified with "show ip ospf neighbor" and "show ip route ospf".

### 3. STP — Rapid PVST+
- RSTP (Rapid PVST+) enabled across all switches
- Core-MLS set as Root Bridge (priority 4096) for all VLANs
- Access-SW1 configured as secondary root for VLAN 10/20
- PortFast + BPDU Guard enabled on all access ports

### 4. ACLs & Security
- Extended ACL blocking HR VLAN from accessing Servers VLAN
- Extended ACL blocking Branch network from Management VLAN
- ACLs applied inbound on appropriate SVIs and WAN interfaces
- Verified with "show access-lists" and targeted ping tests

### 5. DHCP
- DHCP pools configured on HQ-RTR for all 4 VLANs
- Excluded address ranges for network infrastructure
- IP helper-address configured on Core-MLS SVIs for relay
- Verified with "show ip dhcp binding"

### 6. NAT (PAT / Overload)
- NAT overload configured on Branch-RTR for internet simulation
- Inside/outside interfaces designated
- Standard ACL used to define NAT-eligible traffic
- Default route pointing toward ISP
- Verified with "show IP nat translations"

---

## 🔍 Verification Commands

# VLANs
show vlan brief
show interfaces trunk

# STP
show spanning-tree vlan 10
show spanning-tree summary

# OSPF
show ip ospf neighbor
show ip route ospf
show ip protocols

# DHCP
show ip dhcp binding
show ip dhcp pool

# ACLs
show access-lists
show ip interface vlan 10

# NAT
show ip nat translations
show ip nat statistics

# End-to-End Tests
ping 172.16.0.1 source vlan 20          ! HQ IT → Branch (should succeed)
ping 10.30.30.1 source vlan 10          ! HR → Servers (should FAIL — ACL)
ping 10.40.40.1 source 172.16.0.10     ! Branch → Management (should FAIL — ACL)
```

---

📁 Repository Structure

---
enterprise-campus-network/
│
├── README.md                  ← You are here
│
├── configs/
│   ├── HQ-RTR.txt             ← HQ Router full config
│   ├── Branch-RTR.txt         ← Branch Router full config
│   ├── Core-MLS.txt           ← Core Multilayer Switch config
│   ├── Access-SW1.txt         ← Access Switch 1 config
│   └── Access-SW2.txt         ← Access Switch 2 config
│
└── screenshots/
    ├── topology.png            ← NetSim topology screenshot
    ├── ospf-neighbors.png      ← show ip ospf neighbor output
    ├── routing-table.png       ← show ip route output
    ├── stp-root.png            ← show spanning-tree output
    ├── acl-deny.png            ← Ping test showing ACL deny
    └── nat-translations.png    ← show ip nat translations output
---

---

## 🚀 How to Recreate This Lab

1. Open **NetSim** and create a new topology
2. Add devices: 2 Routers, 1 Layer 3 Switch, 2 Layer 2 Switches
3. Connect as shown in the topology diagram above
4. Apply configs from the `/configs` folder to each device in order:
   - Start with switches (VLANs + STP)
   - Then Core-MLS (SVIs + OSPF)
   - Then HQ-RTR (OSPF + DHCP + ACL)
   - Finally Branch-RTR (OSPF + NAT)
5. Run verification commands and capture screenshots

---

## 📚 References

- [Cisco CCNA 200-301 Official Cert Guide](https://www.ciscopress.com)
- [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-15-4m-t/products-command-reference-list.html)
- [NetSim by Boson](https://www.boson.com/netsim-cisco-network-simulator)

---

## 👤 Author

**Alex Tchewedjou**
Network Engineer  and Cybersecurity Analyst

🔗 [LinkedIn](www.linkedin.com/in/alex-tchewedjou-6a2b051a3)
🐙 [GitHub](https://github.com/AlexTchewedjou)

Built as an advanced network engineering showcase project.
Feel free to fork, star ⭐, or reach out with questions!
