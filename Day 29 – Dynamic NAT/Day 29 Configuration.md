# Day 29 – Dynamic NAT – Configuration.md
*Clean, CCNA‑ready configuration for **Dynamic NAT** (1:1 temporary mapping from a public IP pool). This file focuses ONLY on Dynamic NAT. PAT (overload) comes in Day 30.*

---
# 📘 1) Scenario Overview
BlueWave University wants internal users to reach the Internet using a **pool of public IPs**. Each internal connection temporarily gets a **unique public IP** from the pool (1:1), released when idle.

**Inside LAN:** 192.168.20.0/24 (Gig0/0)  
**Outside/WAN:** 203.0.113.0/29 on Gig0/1  
**Public pool available to use:** 203.0.113.2 – 203.0.113.6  

**Goal:**
- Mark inside/outside interfaces
- Define an ACL to select inside traffic
- Create a NAT **pool** of public IPs
- Bind ACL to the pool
- Verify translations

> Use Dynamic NAT when you need true 1:1 public IPs for flows but don’t require a fixed/static mapping per host. If you only have **one** public IP and many users, use **PAT** (Day 30).

---
# 🛠 2) Configure Inside/Outside Interfaces
```bash
configure terminal
interface gig0/0
 description Inside LAN
 ip address 192.168.20.1 255.255.255.0
 ip nat inside
!
interface gig0/1
 description ISP / WAN
 ip address 203.0.113.1 255.255.255.248
 ip nat outside
end
```

---
# 🛠 3) Define the Inside Match ACL
Select which traffic should be translated (the inside subnet):
```bash
configure terminal
access-list 101 permit ip 192.168.20.0 0.0.0.255 any
end
```

---
# 🛠 4) Create the Public NAT Pool
Public pool range: **203.0.113.2 – 203.0.113.6**, mask **/29** (= 255.255.255.248)
```bash
configure terminal
ip nat pool BW_POOL 203.0.113.2 203.0.113.6 netmask 255.255.255.248
end
```

---
# 🛠 5) Bind ACL to the Pool (Dynamic NAT Rule)
```bash
configure terminal
ip nat inside source list 101 pool BW_POOL
end
```

Effect:
- Each new flow from 192.168.20.0/24 gets **one free public IP** from `BW_POOL`.
- If all 5 addresses are in use, additional new sessions are **dropped** until one becomes free.

> If you prefer to **overflow** to PAT (using the outside interface IP) when the pool is exhausted, you can add:  
> `ip nat inside source list 101 interface gig0/1 overload`  
> (That mixes dynamic NAT + PAT, but pure Dynamic NAT is the configuration above.)

---
# 🧪 6) Verification & Troubleshooting

### Show translation table
```bash
show ip nat translations
```
You should see dynamic entries like:
```
Pro  Inside local       Inside global      Outside local      Outside global
---  192.168.20.50     203.0.113.3        142.250.150.78     142.250.150.78
```

### Show NAT statistics and pool usage
```bash
show ip nat statistics
```
Look for: `Pool BW_POOL` with total, allocated, and misses.

### Interface roles
```bash
show ip interface gig0/0 | include NAT
show ip interface gig0/1 | include NAT
```
Expected: `IP nat inside` and `IP nat outside`.

### Common ping/web tests (from an inside host)
- `ping 8.8.8.8`
- Open a website and re‑check `show ip nat translations`.

---
# 🔍 7) Notes & Best Practices
- Dynamic NAT consumes one public IP **per active inside host/flow**.
- If you have fewer public IPs than inside users, consider **PAT** (Day 30).
- Ensure your **default route** on the NAT router points to the ISP next‑hop.
- Make sure return routing from the ISP reaches your NAT device.
- Combine with **ACLs/Firewall** to restrict inbound traffic as needed (Dynamic NAT itself doesn’t open inbound to inside hosts).

---
# ✅ 8) Final Summary
This file delivers a working **Dynamic NAT** setup:
- Inside/Outside roles ✅
- ACL match ✅
- NAT Pool ✅
- Binding rule ✅
- Verification ✅

Proceed to **Day 30 – PAT (NAT Overload)** for scalable Internet access using a single public IP.
