# Day 25 – HSRP – Basic Configuration.md
*Essential Hot Standby Router Protocol (HSRP) configuration to get you started. Packet Tracer does **not** support HSRP. use GNS3 instad*

---
# 📘 1) Scenario Overview
Two routers provide a **redundant default gateway** for the same VLAN/subnet.

- Subnet: **10.1.1.0/24**
- Client default gateway (Virtual IP): **10.1.1.254**
- **R1** preferred Active → 10.1.1.1/24 (G0/0)
- **R2** Standby → 10.1.1.2/24 (G0/0)
- HSRP Group: **1**

Clients use **10.1.1.254** as the gateway. If R1 fails, R2 takes over automatically.

---
# 🛠 2) R1 – Preferred Active Gateway
```bash
configure terminal
interface gig0/0
 ip address 10.1.1.1 255.255.255.0
 ! Create HSRP group 1 with the virtual IP
 standby 1 ip 10.1.1.254
 ! Make R1 the preferred Active (higher priority)
 standby 1 priority 110
 ! Allow R1 to reclaim Active role when it returns
 standby 1 preempt
end
```

---
# 🛠 3) R2 – Standby Gateway
```bash
configure terminal
interface gig0/0
 ip address 10.1.1.2 255.255.255.0
 standby 1 ip 10.1.1.254
 standby 1 priority 100
 standby 1 preempt
end
```

---

# 🧪 4) Verification (on either router)
```bash
show standby brief
show standby
show standby gig0/0
```
Expected highlights:
- One router **Active**, the other **Standby** for group **1**
- **Virtual IP 10.1.1.254**
- **Active router** has the **virtual MAC** (e.g., 0000.0c07.ac01)

**Failover test (lab only):**
```bash
configure terminal
interface g0/0
 shutdown
```
The peer should assume **Active** within a second or two.

---
# 📌 5) Notes & Best Practices
- Use the **same HSRP group ID** and **virtual IP** on both routers.
- HSRP uses UDP/1985 and announces to **224.0.0.2** (all routers on subnet).
- For load‑sharing, you can use **multiple HSRP groups** with different virtual IPs (beyond this basic file).
- **Packet Tracer does not support HSRP** → use GNS3/EVE‑NG/CML/real gear.

---
# ✅ 6) Summary
This minimal config provides:
- Virtual default gateway (**10.1.1.254**)
- Active/Standby roles 
- Verification commands to confirm state and test switchover
