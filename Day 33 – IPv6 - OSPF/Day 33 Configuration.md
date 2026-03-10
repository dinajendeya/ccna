# Day 33 – OSPFv3 – Configuration.md
*CCNA configuration for IPv6 OSPFv3 (link‑local adjacencies, area assignment, RID configuration, DR/BDR tuning, and verification).*

---
# 📘 1) Scenario Overview
Three routers interconnected:
```
R1 ----- R2 ----- R3
```
IPv6 LANs:
- R1 LAN: 2001:DB8:10::/64
- R2 LAN: 2001:DB8:20::/64
- R3 LAN: 2001:DB8:30::/64

Link networks:
- R1↔R2: 2001:DB8:12::/64
- R2↔R3: 2001:DB8:23::/64

Goal:
- Enable OSPFv3
- Use per-interface activation
- Form neighbor adjacencies via link-local
- Configure router IDs
- Verify routing

---
# 🛠 2) Enable IPv6 + Configure Router ID
## R1
```bash
configure terminal
ipv6 unicast-routing
ipv6 router ospf 1
 router-id 1.1.1.1
exit
```
## R2
```bash
configure terminal
ipv6 unicast-routing
ipv6 router ospf 1
 router-id 2.2.2.2
exit
```
## R3
```bash
configure terminal
ipv6 unicast-routing
ipv6 router ospf 1
 router-id 3.3.3.3
exit
```

---
# 🛠 3) IPv6 Addressing
## R1
```bash
interface gig0/0
 ipv6 address 2001:DB8:10::1/64
 ipv6 address FE80::1 link-local
!
interface gig0/1
 ipv6 address 2001:DB8:12::1/64
 ipv6 address FE80::1:1 link-local
```
## R2
```bash
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
```
## R3
```bash
interface gig0/0
 ipv6 address 2001:DB8:30::1/64
 ipv6 address FE80::3 link-local
!
interface gig0/1
 ipv6 address 2001:DB8:23::2/64
 ipv6 address FE80::3:1 link-local
```

---
# 🛠 4) Enable OSPFv3 on Interfaces
## R1
```bash
interface gig0/0
 ipv6 ospf 1 area 0
interface gig0/1
 ipv6 ospf 1 area 0
```
## R2
```bash
interface gig0/0
 ipv6 ospf 1 area 0
interface gig0/1
 ipv6 ospf 1 area 0
interface gig0/2
 ipv6 ospf 1 area 0
```
## R3
```bash
interface gig0/0
 ipv6 ospf 1 area 0
interface gig0/1
 ipv6 ospf 1 area 0
```

---
# 🛠 5) Optional DR/BDR Priority Tuning
```bash
interface gig0/1
 ipv6 ospf priority 200   ! R2 becomes DR
```
```bash
interface gig0/1
 ipv6 ospf priority 150   ! R1 becomes BDR
```
```bash
interface gig0/1
 ipv6 ospf priority 0     ! R3 cannot be DR/BDR
```

---
# 🧪 6) Verification Commands
### Check neighbors
```bash
show ipv6 ospf neighbor
```
### Check OSPF routes
```bash
show ipv6 route ospf
```
### Check OSPF interfaces
```bash
show ipv6 ospf interface gig0/1
```
### Check OSPF process
```bash
show ipv6 ospf
```
### Check LSDB
```bash
show ipv6 ospf database
```
### Ping using link-local (specify interface)
```bash
ping ipv6 FE80::2:1%gig0/1
```

---
# 🏁 7) Final Summary
This configuration includes:
- IPv6 enablement
- Router IDs
- Per-interface OSPFv3 activation
- DR/BDR tuning
- Full verification commands

