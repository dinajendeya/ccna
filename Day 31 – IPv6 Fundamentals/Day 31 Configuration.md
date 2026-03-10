# Day 31 – IPv6 Fundamentals – Configuration.md
*CCNA IPv6 interface, routing, SLAAC, and verification commands.*

---
# 📘 1) Scenario Overview
BlueWave University begins enabling IPv6 across the network.

IPv6 Prefixes:
- **LAN VLAN10:** `2001:DB8:10::/64`  
- **LAN VLAN20:** `2001:DB8:20::/64`  
- **Router Link-Local:** automatically generated (FE80::/10)  
- **Router Global Addresses:** manually configured

Router Interfaces:
- Gig0/0 → VLAN10 LAN
- Gig0/1 → VLAN20 LAN
- Gig0/2 → Upstream router (future routing days)

Goal:
- Enable IPv6 on router
- Assign IPv6 addresses (manual + SLAAC)
- Verify LLAs (link-local addresses)
- Enable Router Advertisements (RA)
- Basic host auto‑addressing

---
# 🛠 2) Enable IPv6 Routing (Required on Cisco IOS)
```bash
configure terminal
ipv6 unicast-routing
end
```

This allows the router to forward IPv6 packets.

---
# 🛠 3) Configure IPv6 on LAN Interface – VLAN10
```bash
configure terminal
interface gig0/0
 description VLAN10 - IPv6 LAN
 ipv6 address 2001:DB8:10::1/64
 ipv6 address FE80::1 link-local
 ipv6 enable
end
```

Notes:
- Global IPv6: `2001:DB8:10::1/64`
- Manual link‑local: `FE80::1` (optional but common for easy routing)
- `ipv6 enable` ensures LLAs even if no global address exists.

---
# 🛠 4) Configure IPv6 on LAN Interface – VLAN20
```bash
configure terminal
interface gig0/1
 description VLAN20 - IPv6 LAN
 ipv6 address 2001:DB8:20::1/64
 ipv6 address FE80::2 link-local
 ipv6 enable
end
```

---
# 🛠 5) SLAAC (Stateless Auto‑Configuration) on LAN
Router Advertisements (RA) are enabled automatically when IPv6 is configured.

On hosts: they generate addresses like:
```
2001:DB8:10::ABCD:1234:5678:9ABC  (randomized IID)
```
---
# 🛠 6) Verify IPv6 on Router
### ✔ Show IPv6 interface details
```bash
show ipv6 interface gig0/0
```
Shows:
- Link-local
- Global unicast
- RA settings

### ✔ Check IPv6 neighbors (NDP)
```bash
show ipv6 neighbors
```

### ✔ Check IPv6 routing table
```bash
show ipv6 route
```
You should see connected routes:
```
C   2001:DB8:10::/64 is directly connected, Gig0/0
L   2001:DB8:10::1/128 is directly connected, Gig0/0
```

---
# 🧪 7) Host Verification (Windows / Linux)
### Windows:
```powershell
ipconfig
```
Should see:
- IPv6 global address
- Link-local FE80::
- Default gateway (router’s link-local)

### Linux/macOS:
```bash
ip -6 addr
ip -6 route
```

---
# 🔍 10) Common IPv6 Issues & Fixes
| Issue | Cause | Fix |
|-------|--------|------|
| No IPv6 global address on hosts | RA disabled | Ensure `ipv6 unicast-routing` & interface IPv6 configured |
| Ping fails | Using wrong ping tool | Use `ping ipv6` or `ping6` |
| Wrong gateway | Hosts use LLA as gateway | Normal behavior — LLAs used for routing |
| Duplicate address | MAC-based IIDs conflict | DAD fails → host generates a random IID |

---
# 🏁 11) Final Summary
This configuration file includes:
- IPv6 routing enablement
- IPv6 addressing on interfaces
- Global + link‑local configuration
- SLAAC setup
- DHCPv6 (stateless & stateful)
- Verification commands
- Host testing

This completes **Day 31 – IPv6 Fundamentals – Configuration.md**.
Next: **Day 32 – IPv6 Static Routing**.
