# Day 24 – OSPF (Part 2) – Configuration.md
*This file contains ONLY configurations for OSPF on Broadcast Multi‑Access Networks, including DR/BDR tuning, loopback behavior, interface priority, and multicast verification. No overlap with Day 23 (Point‑to‑Point).* 

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Routers: **R1, R2, R3** on the same Ethernet segment (Broadcast network)
Network: **192.168.50.0/24** on VLAN 50

Goal:
- Configure OSPF on a broadcast network
- Control DR/BDR elections using interface priority
- Configure loopback interfaces for stable Router IDs
- Verify OSPF multicasts (224.0.0.5 & 224.0.0.6)
- Demonstrate Router ID selection

Topology:
```
      +------ R1 ------+
      |  (DR expected) |
      +----------------+
         |   |   |
      ---+---+---+----------------  Broadcast Segment
         |   |   |
      +------ R2 ------+
      | (BDR expected) |
      +----------------+
         |   |   |
      +------ R3 ------+
      | (DROther)      |
      +----------------+
```

---
# 🛠 2. Configure Loopback for Router ID Stability
Loopbacks ensure a stable, always‑up Router ID.

## On R1:
```bash
configure terminal
interface loopback0
 ip address 1.1.1.1 255.255.255.255
end
```

## On R2:
```bash
configure terminal
interface loopback0
 ip address 2.2.2.2 255.255.255.255
end
```

## On R3:
```bash
configure terminal
interface loopback0
 ip address 3.3.3.3 255.255.255.255
end
```

OSPF will automatically choose the highest loopback IP as Router ID.

---
# 🛠 3. Enable OSPF on Broadcast Interfaces
Each router uses the same OSPF process and area.

## On All Routers:
```bash
configure terminal
router ospf 1
 network 192.168.50.0 0.0.0.255 area 0
 network 1.1.1.1 0.0.0.0 area 0
 network 2.2.2.2 0.0.0.0 area 0
 network 3.3.3.3 0.0.0.0 area 0
end
```

> Note: OSPF auto-detects broadcast network type on Ethernet.

---
# 🛠 4. Configure OSPF Interface Priority (DR/BDR Control)
OSPF elects DR/BDR based on **priority → Router ID**.

## Make R1 the DR:
```bash
interface gig0/0
 ip ospf priority 200
```

## Make R2 the BDR:
```bash
interface gig0/0
 ip ospf priority 150
```

## Make R3 a DROther:
```bash
interface gig0/0
 ip ospf priority 0
```
(Priority 0 = cannot become DR or BDR.)

---
# 🛠 5. Optional: Set OSPF Network Type Manually
Ethernet defaults to broadcast, but can be enforced:
```bash
interface gig0/0
 ip ospf network broadcast
```

---
# 🛠 6. Adjust Reference Bandwidth for Accurate Cost
Modern networks use >100 Mbps links.
```bash
router ospf 1
 auto-cost reference-bandwidth 10000
end
```

---
# 🛠 7. Passive Interfaces (LAN Users Should Not Receive OSPF)
On routers with user-facing interfaces:
```bash
router ospf 1
 passive-interface gig0/1
end
```

---
# 🧪 8. Verification Commands
### ✔ Check DR/BDR Election
```bash
show ip ospf neighbor
```
Expected Example:
```
Neighbor ID     Pri   State           Interface
2.2.2.2         150   FULL/BDR        Gig0/0
1.1.1.1         200   FULL/DR         Gig0/0
3.3.3.3           0   2WAY/DROTHER    Gig0/0
```

### ✔ Check Router ID
```bash
show ip ospf
```
Expected:
```
Router ID 1.1.1.1 (for R1)
```

### ✔ Check Multicast Usage
```bash
show ip ospf interface gig0/0
```
Expected to show:
```
Multicast: 224.0.0.5, 224.0.0.6
Network Type: BROADCAST
```

### ✔ Check LSDB
```bash
show ip ospf database
```

### ✔ Verify Loopback Advertisement (/32)
```bash
show ip route ospf
```
Example:
```
O 1.1.1.1/32 [110/2] via 192.168.50.1
```

---
# 🔍 9. Expected Behavior in Real Networks
- R1 becomes DR, R2 becomes BDR
- R3 always DROther
- Loopbacks appear as /32 host routes
- Multicast 224.0.0.5 used for Hellos
- Multicast 224.0.0.6 used for DR/BDR
- LSAs flood only to DR/BDR → reduced traffic

---
# ⚠ Common OSPF Issues & Fixes
| Issue | Symptom | Fix |
|--------|--------|-----|
| Wrong priority | Unexpected DR wins | Set `ip ospf priority` properly |
| Missing loopback | Unstable Router ID | Add Loopback0 |
| Different areas | No adjacency | Ensure all routers use area 0 |
| Subnet mismatch | No neighbors | Match mask + network |
| Multicast blocked | Adjacency fails | Allow 224.0.0.5 & .6 |

---
# 🏁 10. Final Summary
This file includes:
- Loopback configuration for Router ID
- OSPF on broadcast networks
- DR/BDR priority tuning
- Multicast verification
- LSDB inspection
- Interface priority control

This completes OSPF Part 2 configuration. Next is **Day 25 – FHRP (HSRP/VRRP/GLBP)**.
