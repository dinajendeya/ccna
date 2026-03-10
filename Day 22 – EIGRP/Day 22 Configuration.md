# Day 22 – EIGRP – Configuration.md
*Complete EIGRP configuration with explanations, verification, and real‑world scenario.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Routers: **R1, R2, R3**
Routing Protocol: **EIGRP for IPv4**
Autonomous System (AS): **100**

Network Topology:
```
R1 --- R2 --- R3
```
Connected networks:
- R1 LAN: 192.168.10.0/24
- R2 LAN: 192.168.20.0/24
- R3 LAN: 192.168.30.0/24
- R1–R2 Link: 10.1.12.0/30
- R2–R3 Link: 10.1.23.0/30

Goal:
- Enable EIGRP on all routers
- Form neighbor adjacencies
- Advertise all networks
- Prevent updates from flooding LAN users (passive-interface)
- Verify routing table

---
# 🛠 2. Basic EIGRP Configuration (All Routers)
EIGRP must be configured with the **same AS number** for adjacency.

---
## 🔹 Router R1
```bash
configure terminal
router eigrp 100
 network 192.168.10.0 0.0.0.255
 network 10.1.12.0 0.0.0.3
 no auto-summary
 passive-interface gig0/0
end
```

---
## 🔹 Router R2
```bash
configure terminal
router eigrp 100
 network 192.168.20.0 0.0.0.255
 network 10.1.12.0 0.0.0.3
 network 10.1.23.0 0.0.0.3
 no auto-summary
 passive-interface gig0/0
end
```

---
## 🔹 Router R3
```bash
configure terminal
router eigrp 100
 network 192.168.30.0 0.0.0.255
 network 10.1.23.0 0.0.0.3
 no auto-summary
 passive-interface gig0/0
end
```

---
# 🛠 3. Optional: Advertise a Default Route (Gateway Router Only)
If R1 has Internet access:

```bash
configure terminal
ip route 0.0.0.0 0.0.0.0 <next-hop>
router eigrp 100

end
```

Effect: EIGRP advertises the default route to all routers.


---
# 🧪 4. Verification Commands
### ✔ Check EIGRP Neighbors
```bash
show ip eigrp neighbors
```
Expected:
- R1 sees R2
- R2 sees R1 & R3
- R3 sees R2

### ✔ Check EIGRP Routes
```bash
show ip route eigrp
```
Expected Example:
```
D 192.168.30.0/24 [90/2172416] via 10.1.23.2
```
`D` = EIGRP route.

### ✔ Check EIGRP Topology Table
```bash
show ip eigrp topology
```
Shows:
- Successor routes
- Feasible successors
- Metrics

### ✔ Debug (Lab Use Only)
```bash
debug eigrp packets
```
⚠ Do NOT use in production.

---
# 🔍 5. Expected Behavior
- EIGRP forms adjacencies instantly
- Routes are exchanged dynamically
- Failures converge rapidly using the DUAL algorithm
- Backup paths (Feasible Successors) activate immediately

---
# ⚠ Common EIGRP Issues & Fixes
| Issue | Symptom | Fix |
|-------|----------|------|
| Wrong AS number | No neighbors form | Match AS on all routers |
| Missing network statements | Routes not learned | Add correct wildcard masks |
| Auto-summary enabled | Incorrect routes | Use `no auto-summary` |
| K-values mismatch | No adjacency | Keep metric defaults |

---
# 🏁 8. Final Summary
This configuration file includes:
- Full EIGRP deployment
- Default route advertisement
- Verification outputs

This file covers **EIGRP only**.
OSPF configuration starts in **Day 23 – OSPF (Part 1)**.
