# Day 26 – Standard ACLs – Configuration.md
*Essential and practical configuration examples for Standard Access Control Lists. This file covers only Standard ACLs. Extended ACLs come in Day 27.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Routers: Cisco IOS (ISR / 2800 / 2900 / 4321 / Packet Tracer)

Network:
- Admin PCs: **10.1.1.10**, **10.1.1.11**
- Student network: **192.168.50.0/24**
- Router VTY access must be restricted
- Students must not access 192.168.100.0/24 (Server LAN)

Goal:
- Create Standard ACLs
- Filter based ONLY on **source IP**
- Apply ACLs correctly at the **destination**
- Protect router VTY lines

---
# 🛠 2. Block a Single Host (Basic Standard ACL)
Block **10.1.1.50** from accessing anything.

```bash
configure terminal
access-list 10 deny 10.1.1.50
access-list 10 permit any
end
```

Apply it inbound on the LAN interface:
```bash
configure terminal
interface gig0/0
 ip access-group 10 in
end
```

---
# 🛠 3. Allow Only Admin PCs to Access the Router (VTY Protection)
Admin PCs allowed:
- 10.1.1.10
- 10.1.1.11

```bash
configure terminal
access-list 12 permit 10.1.1.10
access-list 12 permit 10.1.1.11
access-list 12 deny any
end
```

Apply ACL to VTY lines:
```bash
configure terminal
line vty 0 4
 access-class 12 in
 login local
 transport input ssh
end
```

---
# 🛠 4. Block an Entire Subnet from Reaching the Server LAN
Deny traffic from **192.168.50.0/24** (student VLAN) to servers.

### 1) Create ACL:
```bash
configure terminal
access-list 15 deny 192.168.50.0 0.0.0.255
access-list 15 permit any
end
```

### 2) Apply ACL near the **destination** (server LAN interface):
```bash
interface gig0/1
 ip access-group 15 in
end
```

Explanation:
- Standard ACL → only filters by source IP, so applied near destination.

---
# 🛠 5. Permit Only a Specific Subnet to Send Traffic
Permit **10.10.10.0/24** only.

```bash
configure terminal
access-list 20 permit 10.10.10.0 0.0.0.255
access-list 20 deny any
end
```

Apply to interface:
```bash
interface gig0/2
 ip access-group 20 in
end
```

---
# 🛠 6. Named Standard ACL Example
Same as numbered, but easier to edit.

```bash
configure terminal
ip access-list standard PERMIT_ADMIN
 permit 10.1.1.10
 permit 10.1.1.11
 deny any
end
```

Apply:
```bash
interface gig0/0
 ip access-group PERMIT_ADMIN in
end
```

---
# 🧪 7. Verification Commands
### ✔ Show all ACLs
```bash
show access-lists
```

### ✔ Show ACL applied on interface
```bash
show ip interface gig0/0
```
Look for:
```
Inbound access list is 10
```

---
# 🔍 8. Expected Behavior
- Traffic from blocked source IPs is dropped.
- Only permitted admin IPs can SSH/Telnet.
- Students cannot access servers.
- ACLs process top‑down; first match wins.

---
# ⚠ Common Standard ACL Mistakes & Fixes
| Issue | Cause | Fix |
|-------|-------|-----|
| ACL applied near source | Overblocking | Apply near **destination** |
| No final permit | Implicit deny drops all | Add `permit any` at end |
| Wrong wildcard | Traffic not matched | Use correct wildcard mask |
| Applied wrong direction | ACL doesn’t trigger | Choose **inbound/outbound** correctly |

---
# 🏁 9. Final Summary
This file includes:
- Basic host-blocking ACLs
- Subnet blocking
- VTY protection
- Named + numbered ACL examples
- Correct interface placement
- Full verification commands

This completes **Standard ACLs** for Day 26.
Extended ACLs come next in **Day 27**.
