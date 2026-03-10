# Day 21 – Dynamic Routing – Configuration.md
*This configuration file focuses ONLY on RIP (Routing Information Protocol) as the first dynamic routing protocol introduced in Day 21.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Routers: **R1, R2, R3** using IPv4

Goal:
- Enable dynamic routing using **RIP v2**
- Allow routers to automatically exchange network information
- Demonstrate basic RIP operations
- Avoid auto-summary issues

Network Topology:
```
R1 --- R2 --- R3
```
Connected networks:
- R1 LAN: 192.168.10.0/24
- R2 LAN: 192.168.20.0/24
- R3 LAN: 192.168.30.0/24
- R1–R2 Link: 10.0.12.0/30
- R2–R3 Link: 10.0.23.0/30

We want RIP to learn all networks dynamically.

---
# 🛠 2. Enable RIP v2 on All Routers
RIP requires enabling routing and adding network statements.

## 🔹 Router R1
```bash
configure terminal
router rip
 version 2
 no auto-summary
 network 192.168.10.0
 network 10.0.12.0
end
```

## 🔹 Router R2
```bash
configure terminal
router rip
 version 2
 no auto-summary
 network 192.168.20.0
 network 10.0.12.0
 network 10.0.23.0
end
```

## 🔹 Router R3
```bash
configure terminal
router rip
 version 2
 no auto-summary
 network 192.168.30.0
 network 10.0.23.0
end
```

---

# 🛠 3. Advertising a Default Route (Optional)
If R1 is the gateway to the Internet:

## On R1:
```bash
configure terminal
ip route 0.0.0.0 0.0.0.0 <next-hop>
router rip
 default-information originate
end
```

Effect: other routers learn the default route via RIP.

---
# 🧪 4. Verification Commands
### ✔ Check RIP neighbors (RIP uses broadcast/multicast, not neighbors)
RIP does not form “neighbors” like OSPF/EIGRP.
But we can view routing table entries.

### ✔ Check Routing Table
```bash
show ip route rip
```
Expected:
```
R 192.168.30.0/24 [120/1] via 10.0.23.2
R 192.168.10.0/24 [120/1] via 10.0.12.1
```

### ✔ Check RIP Database
```bash
show ip rip database
```
Shows all learned networks.

### ✔ Debug RIP (for lab use only)
```bash
debug ip rip
```
**CAUTION:** Do not use in production.

---
# 🔍 5. Expected Real‑World Behavior
- Routers automatically learn remote networks
- Topology changes trigger automatic updates
- Convergence is slow but predictable
- Best suited for small networks

---
# ⚠ Common RIP Issues & Fixes
| Issue | Symptom | Fix |
|-------|----------|------|
| Auto-summary enabled | Wrong subnet routes | Use `no auto-summary` |
| Missing network statement | Route not learned | Add correct network | 
| Using RIP v1 by mistake | No VLSM support | Use `version 2` |
| Incorrect masks on interfaces | Ignored routes | Verify interface IPs |

---
# 🏁 6. Final Summary
This file includes only RIP configurations:
- RIP v2 setup
- network statements
- passive interfaces
- default route advertisement
- verification commands

It does **not** include EIGRP or OSPF (covered in Day 23 and Day 24).

RIP dynamic routing is now fully configured.
