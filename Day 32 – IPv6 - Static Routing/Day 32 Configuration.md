# Day 32 – IPv6 Static Routing – Configuration.md
*CCNA configuration of IPv6 static routing, including global unicast next‑hop routes, link‑local next‑hop routes, default routes (::/0), and floating static routes.*

---
# 📘 1) Scenario Overview
BlueWave University uses **three routers** connected in a chain:

```
R1 ---- R2 ---- R3
```

IPv6 Networks:
- **R1 LAN:** 2001:DB8:10::/64
- **R2 LAN:** 2001:DB8:20::/64
- **R3 LAN:** 2001:DB8:30::/64

Router Link Interfaces:
- **R1 ↔ R2:** 2001:DB8:12::/64
- **R2 ↔ R3:** 2001:DB8:23::/64

Goal:
- Configure IPv6 addresses on interfaces
- Use IPv6 static routes (global next-hop)
- Use IPv6 static routes (link-local next-hop)
- Add default route (::/0) on R1
- Add floating static routes
- Verify using IPv6 commands

---
# 🛠 2) Step 1 – Configure IPv6 on Each Router Interface
## R1
```bash
configure terminal
interface gig0/0
 ipv6 address 2001:DB8:10::1/64
 ipv6 address FE80::1 link-local
!
interface gig0/1
 ipv6 address 2001:DB8:12::1/64
 ipv6 address FE80::1:1 link-local
end
```

## R2
```bash
configure terminal
interface gig0/0
 ipv6 address 2001:DB8:20::1/64
 ipv6 address FE80::2 link-local
!
interface gig0/1
 ipv6 address 2001:DB8:12::2/64
 ipv6 address FE80::2:1 link-local
!
interface gig0/2
 ipv6 address 2001:DB8:23::1/64
 ipv6 address FE80::2:2 link-local
end
```

## R3
```bash
configure terminal
interface gig0/0
 ipv6 address 2001:DB8:30::1/64
 ipv6 address FE80::3 link-local
!
interface gig0/1
 ipv6 address 2001:DB8:23::2/64
 ipv6 address FE80::3:1 link-local
end
```

Enable IPv6 routing globally on all routers:
```bash
configure terminal
ipv6 unicast-routing
end
```

---
# 🛠 3) Step 2 – IPv6 Static Routes (Global Unicast Next-Hop)
## R1 → reach R2 LAN & R3 LAN
```bash
configure terminal
ipv6 route 2001:DB8:20::/64 2001:DB8:12::2
ipv6 route 2001:DB8:30::/64 2001:DB8:12::2
end
```

## R3 → reach R1 LAN & R2 LAN
```bash
configure terminal
ipv6 route 2001:DB8:20::/64 2001:DB8:23::1
ipv6 route 2001:DB8:10::/64 2001:DB8:23::1
end
```

## R2 → reach R1 LAN & R3 LAN (global next-hop)
```bash
configure terminal
ipv6 route 2001:DB8:10::/64 2001:DB8:12::1
ipv6 route 2001:DB8:30::/64 2001:DB8:23::2
end
```

---
# 🛠 4) Step 3 – IPv6 Static Routes (Link‑Local Next-Hop)
Link‑local routes **must specify the exit interface**.

## R1
```bash
configure terminal
ipv6 route 2001:DB8:20::/64 FE80::2:1 gig0/1
ipv6 route 2001:DB8:30::/64 FE80::2:1 gig0/1
end
```

## R3
```bash
configure terminal
ipv6 route 2001:DB8:10::/64 FE80::2:2 gig0/1
ipv6 route 2001:DB8:20::/64 FE80::2:2 gig0/1
end
```

## R2
```bash
configure terminal
ipv6 route 2001:DB8:10::/64 FE80::1:1 gig0/1
ipv6 route 2001:DB8:30::/64 FE80::3:1 gig0/2
end
```

---
# 🛠 5) Step 4 – IPv6 Default Route (::/0)
Usually added on the **edge router**.

### Example: R1 uses R2 as default gateway
```bash
configure terminal
ipv6 route ::/0 FE80::2:1 gig0/1
end
```

---
# 🛠 6) Step 5 – Floating Static Routes (Backup Routes)
Floating routes must have **higher administrative distance**.

Example: R2 uses R3 as a backup path to R1 LAN
```bash
configure terminal
ipv6 route 2001:DB8:10::/64 FE80::3:1 gig0/2 50
end
```

Primary static route AD = **1**  
Floating route AD = **50** → only used if primary fails.

---
# 🧪 7) Verification Commands
### ✔ Routing Table
```bash
show ipv6 route
```
Look for:
```
S   2001:DB8:30::/64 [1/0] via FE80::2:1, Gig0/1
```

### ✔ Neighbor Discovery Table
```bash
show ipv6 neighbors
```
Shows L2 reachability.

### ✔ Interface IPv6 Summary
```bash
show ipv6 interface brief
```

### ✔ Ping IPv6
```bash
ping ipv6 2001:DB8:30::1
```

### ✔ Trace IPv6
```bash
traceroute ipv6 2001:DB8:20::1
```

---
# 🏁 8) Final Summary
This configuration file provides:
- IPv6 interface addressing
- Link‑local & global addressing
- Static routes (global next-hop)
- Static routes (link-local next-hop + interface)
- IPv6 default routes
- Floating static routes
- Full verification commands

This completes **Day 32 – IPv6 Static Routing – Configuration.md**.
Next: **Day 33 – IPv6 OSPFv3**.
