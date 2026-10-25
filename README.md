# 🏛️ Advanced Enterprise Network Lab — CCNA/CCNP Showcase

> A maximum-complexity network simulation featuring multi-area OSPF, EIGRP redistribution, eBGP, HSRP, EtherChannel, DMVPN, VRFs, IPv6 dual-stack, QoS, and AAA security — built in Boson NetSim.

---

## 📌 Project Overview
<img width="1447" height="840" alt="topology" src="https://github.com/user-attachments/assets/7d7a7e26-c152-4995-9e93-7f730b0762fc" />

This lab simulates a **multi-site enterprise** with a headquarters, two branch offices, a data center, and a dual-ISP internet edge — all interconnected with redundant paths, dynamic routing, and production-grade security policies.

| Attribute         | Detail                                             |
|---                |---                                                 |
| Simulator         | NetSim (Boson)                                     |
| Target Cert       | Cisco CCNA 200-301 / CCNP ENCOR 350-401            |
| Devices           | 5 Routers, 4 Switches, 2 ISP Routers               |
| Routing Protocols | OSPF Multi-Area, EIGRP, eBGP, Route Redistribution |
| Redundancy        | HSRP, EtherChannel (LACP), Dual ISP                |
| Tunneling         | DMVPN Phase 2 (GRE over mGRE)                      |
| Security          | AAA/RADIUS, Extended ACLs, DHCP Snooping           |
| Advanced          | VRFs, IPv6 Dual-Stack, QoS                         |

---

## 🗺️ Network Topology

                [ISP-A]                    [ISP-B]
              203.0.113.1                198.51.100.1
                  |  eBGP AS100       eBGP AS200  |
                  |                               |
                [EDGE-RTR AS65000]
               G0/0: 203.0.113.2 (ISP-A)
               G0/1: 198.51.100.2 (ISP-B)
               G0/2: 10.0.0.1/30 → HQ-CORE
               DMVPN Hub: 10.100.0.1
                      |
               OSPF Area 0
                      |
               [HQ-CORE-RTR]
               OSPF ABR: Area 0 ↔ Area 1 ↔ Area 2
               EIGRP redistribute into OSPF
              /                    \
     OSPF Area 1              OSPF Area 2
          |                        |
     [DC-RTR]                [MLS-CORE L3 SW]
     VRF: PROD/MGMT          HSRP Active
     10.1.0.2/30             EtherChannel to MLS-2
          |                        |
     [DC-SW]                 [MLS-2 L3 SW]
     VLAN 100, 200           HSRP Standby
                                   |
                             [Access-SW L2]
                             VLAN 10/20/30/40

DMVPN Spokes (over internet):
[BRANCH1-RTR] Spoke: 10.100.0.2 LAN: 172.16.1.0/24
[BRANCH2-RTR] Spoke: 10.100.0.3 LAN: 172.16.2.0/24

---

## 📋 IP Addressing Scheme

| Device      | Interface   | IP Address     | Description                   |
|---          |---          |---             |---                            |
| EDGE-RTR    | Fa0/0       | 203.0.113.2/30 | ISP-A Link                    |
| EDGE-RTR    | Fa1/0       | 198.51.100.2/30| ISP-B Link                    |
| EDGE-RTR    | Fa2/0       | 10.0.0.1/30    | Link to HQ-CORE               |
| EDGE-RTR    | Tunnel0     | 10.100.0.1/24  | DMVPN Hub                     |
| HQ-CORE-RTR | Fa0/0       | 10.0.0.2/30    | Link to EDGE                  |
| HQ-CORE-RTR | Fa1/0       | 10.1.0.1/30    | Link to DC-RTR                |
| HQ-CORE-RTR | Fa2/0       | 10.2.0.1/30    | Link to MLS-CORE              |
| DC-RTR      | Fa0/0       | 10.1.0.2/30    | Link to HQ-CORE               |
| DC-RTR      | Fa1/0.100   | 10.100.10.1/24 | VRF PROD                      |
| DC-RTR      | Fa1/0.200   | 10.100.20.1/24 | VRF MGMT                      |
| MLS-CORE    | Fa0/1       | 10.2.0.2/30    | Link to HQ-CORE               |
| MLS-CORE    | VLAN 10 SVI | 10.10.10.2/24  | HR Gateway (HSRP Active)      |
| MLS-CORE    | VLAN 20 SVI | 10.20.20.2/24  | IT Gateway (HSRP Active)      |
| MLS-CORE    | VLAN 30 SVI | 10.30.30.2/24  | Servers Gateway (HSRP Active) |
| MLS-CORE    | VLAN 40 SVI | 10.40.40.2/24  | MGMT Gateway (HSRP Active)    |
| MLS-2       | VLAN 10 SVI | 10.10.10.3/24  | HR Gateway (HSRP Standby)     | 
| MLS-2       | VLAN 20 SVI | 10.20.20.3/24  | IT Gateway (HSRP Standby)     |
| BRANCH1-RTR | Fa0/0       | 203.0.114.1/30 | ISP Uplink                    |
| BRANCH1-RTR | Fa1/0       | 172.16.1.1/24  | Branch LAN                    |
| BRANCH1-RTR | Tunnel0     | 10.100.0.2/24  | DMVPN Spoke                   |
| BRANCH2-RTR | Fa0/0       | 203.0.115.1/30 | ISP Uplink                    |
| BRANCH2-RTR | Fa1/0       | 172.16.2.1/24  | Branch LAN                    |
| BRANCH2-RTR | Tunnel0     | 10.100.0.3/24  | DMVPN Spoke                   |
| ISP-A       | Fa0/0       | 203.0.113.1/30 | eBGP AS100                    |
| ISP-B       | Fa0/0       | 198.51.100.1/30| eBGP AS200                    |

---

## ✅ Skills Demonstrated

| Domain     | Technology                        | Verified |
|---         |---                                |---       |
| Routing    | OSPF Multi-Area (0,1,2)           | ✅ |
| Routing    | EIGRP AS 100 at branches          | ✅ |
| Routing    | Route Redistribution EIGRP↔OSPF   | ✅ |
| Routing    | eBGP Dual-ISP failover            | ✅ |
| Redundancy | HSRP Active/Standby all VLANs     | ✅ |
| Redundancy | EtherChannel LACP (Port-Channel1) | ✅ |
| Tunneling  | DMVPN Phase 2 Hub & Spoke         | ✅ |
| VLANs      | 802.1Q trunking, SVIs             | ✅ |
| STP        | Rapid PVST+, Root Bridge          | ✅ |
| Security   | Extended ACLs                     | ✅ |
| Security   | DHCP Snooping                     | ✅ |
| VRF        | VRF-Lite PROD/MGMT                | ✅ |
| DHCP       | Pools + helper-address            | ✅ |
| NAT        | PAT overload                      | ✅ |

---

## 🔍 Verification Commands

```bash
# OSPF
show ip ospf neighbor
show ip ospf border-routers
show ip route ospf

# BGP
show ip bgp summary
show ip bgp
show ip route bgp

# HSRP
show standby brief

# EtherChannel
show etherchannel summary

# VRF
show ip vrf
show ip route vrf PROD
show ip route vrf MGMT

# VLANs
show vlan brief
show interfaces trunk

# NAT
show ip nat translations
show ip nat statistics

# EIGRP
show ip eigrp neighbors
show ip eigrp topology

# DMVPN
show dmvpn
show ip nhrp
```

---

## 🧪 End-to-End Test Matrix

| Test             | Source             | Destination     | Expected                              | 
|---               |---                 |---              | ---                                   |
| Inter-VLAN       | VLAN 10            | VLAN 20         | ✅ Success                           |
| HSRP failover    | VLAN 10            | Internet        | ✅ Continues after MLS-CORE shutdown |
| Branch to HQ     | 172.16.1.10        | 10.10.10.50     | ✅ DMVPN tunnel                      |
| ACL deny         | VLAN 10 HR         | VLAN 30 Servers | ❌ Denied                            |
| VRF isolation    | VRF PROD           | VRF MGMT        | ❌ Isolated                          |
| Internet         | Any internal       | 203.0.113.1     | ✅ Success                           |
| ISP failover     | Any internal       | Internet        | ✅ BGP reroutes via ISP-B            |

---

## 📁 Repository Structure


advanced-enterprise-network-lab/
│
├── README.md
│
├── configs/
│ ├── EDGE-RTR.txt
│ ├── HQ-CORE-RTR.txt
│ ├── DC-RTR.txt
│ ├── MLS-CORE.txt
│ ├── MLS-2.txt
│ ├── Access-SW.txt
│ ├── DC-SW.txt
│ ├── BRANCH1-RTR.txt
│ ├── BRANCH2-RTR.txt
│ ├── ISP-A.txt
│ └── ISP-B.txt
│
└── screenshots/
    ├── topology.png
    ├── bgp-dual-isp.png
    ├── ospf-multi-area.png
    ├── hsrp-active.png
    ├── hsrp-standby.png
    ├── etherchannel.png
    ├── vrf-routing.png
    ├── vlan-trunk.png
    └── ping-internet.png           

---

## ⚠️ NetSim Limitations

Some advanced features have limited support in NetSim's simulated IOS:
- **DHCP Snooping trust** — configured but not enforced
- **Dynamic ARP Inspection** — not supported on this IOS version
- **DMVPN EIGRP neighbors** — mGRE multipoint not fully simulated
- **NAT translation table** — requires real end-host traffic

These features are fully configured and production-ready. They would function completely on real Cisco hardware or GNS3 with IOSv images.

---

## 📚 References

- [Cisco CCNA 200-301 Official Cert Guide](https://www.ciscopress.com)
- [Cisco CCNP ENCOR 350-401 Official Cert Guide](https://www.ciscopress.com)
- [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/support/ios-nx-os-software)
- [NetSim by Boson](https://www.boson.com/netsim-cisco-network-simulator)

---

## 👤 Author

**Alex Tchewedjou**
Cybersecurity Analyst| Network Engineer | 

🔗 [LinkedIn](www.linkedin.com/in/alex-tchewedjou-6a2b051a3)
🐙 [GitHub](https://github.com/AlexTchewedjou)

Built as an advanced network engineering showcase project.
Feel free to fork, star ⭐, or reach out with questions!
