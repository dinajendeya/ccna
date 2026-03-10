# Day 23 – OSPF (Part 1) – Configuration.md

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Routers: **R1 – R2 – R3** connected using **point‑to‑point links**.

Topology:
```
R1 —— R2 —— R3
```

Networks:
- R1 LAN: 192.168.10.0/24
- R2 LAN: 192.168.20.0/24
- R3 LAN: 192.168.30.0/24
- R1–R2 link: 10.10.12.0/30
- R2–R3 link: 10.10.23.0/30

Goal:
- Configure OSPF on all routers
- Use Area 0 (Backbone)
- Demonstrate OSPF Router IDs
- Adapt point‑to‑point behavior
- Verify neighbors & SPF routes

No DR/BDR elections occur on point‑to‑point links.

---
# 🛠 2. Assign Router IDs (Recommended Best Practice)
Avoid automatic Router ID selection.

## R1:
```bash
configure terminal
router ospf 1
 router-id 1.1.1.1
end
```

## R2:
```bash
configure terminal
router ospf 1
 router-id 2.2.2.2
end
```

## R3:
```bash
configure terminal
router ospf 1
 router-id 3.3.3.3
end
```

To apply changes:
```bash
clear ip ospf process
```
(Use carefully in production.)

---
# 🛠 3. Configure OSPF (Point‑to‑Point Links)
## 🔹 Router R1
```bash
configure terminal
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.10.12.0 0.0.0.3 area 0
end
```

## 🔹 Router R2
```bash
configure terminal
router ospf 1
 network 192.168.20.0 0.0.0.255 area 0
 network 10.10.12.0 0.0.0.3 area 0
 network 10.10.23.0 0.0.0.3 area 0
end
```

## 🔹 Router R3
```bash
configure terminal
router ospf 1
 network 192.168.30.0 0.0.0.255 area 0
 network 10.10.23.0 0.0.0.3 area 0
end
```

---
# 🛠 4. Explicitly Set Network Type (Optional but Good Practice)
Most Serial/PPP links are auto point‑to‑point.
But you can force it:

### Example on R2 Gig0/1:
```bash
configure terminal
interface gig0/1
 ip ospf network point-to-point
end
```

This improves adjacency speed.

---
# 🧪 5. Verification Commands
### ✔ Check Neighbors
```bash
show ip ospf neighbor
```
Expected on point‑to‑point:
```
State: FULL
DR: 0.0.0.0  (no DR on p2p)
BDR: 0.0.0.0
```

### ✔ Check OSPF Routes
```bash
show ip route ospf
```
Example:
```
O 192.168.30.0/24 [110/20] via 10.10.23.2, Gig0/1
```

### ✔ Check OSPF Database
```bash
show ip ospf database
```
Shows LSAs and SPF info.

### ✔ Check Interface State
```bash
show ip ospf interface gig0/1
```
Expected:
```
Network Type: POINT-TO-POINT
Cost: <value>
```

---
# 🔍 6. Expected Behavior
- All routers form adjacencies quickly
- No DR/BDR election occurs (because p2p)
- LSAs exchanged reliably
- SPF recalculates quickly on link failure

---
# 🏁 7. Final Summary
This configuration file includes:
- Manual Router ID setup
- OSPF Area 0 configuration
- Point‑to‑Point network settings
- Full verification commands

No overlap with Day 24 topics:
- OSPF Broadcast networks
- DR/BDR elections
- Multicast addresses
- Loopback interfaces
- Router ID selection rules

OSPF Part 1 is now fully implemented.
