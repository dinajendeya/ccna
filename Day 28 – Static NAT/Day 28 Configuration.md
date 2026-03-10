# Day 28 – Static NAT – Configuration.md
*Practical, clean, CCNA‑ready Static NAT configuration. This configuration maps one internal private IP to one public IP. Applies to routers, firewalls, or any NAT‑capable Cisco device.*

---
# 📘 1) Scenario Overview
BlueWave University needs to publish a **web server** on the Internet.

Internal server (private): `192.168.10.10`  
Public IP from ISP: `203.0.113.10`  
Inside interface: `Gig0/0` (LAN)  
Outside interface: `Gig0/1` (WAN)  

Goal:
- Create a **1-to-1 Static NAT mapping**
- Allow inbound HTTP/HTTPS
- Ensure return traffic is translated correctly

---
# 🛠 2) Mark Inside & Outside Interfaces
```bash
configure terminal
interface gig0/0
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
!
interface gig0/1
 ip address 203.0.113.1 255.255.255.248
 ip nat outside
end
```

**Why?** NAT requires knowing which interface is “inside” (private) and which is “outside” (public).

---
# 🛠 3) Create Static NAT Mapping (Core Command)
```bash
configure terminal
ip nat inside source static 192.168.10.10 203.0.113.10
end
```

This creates a **permanent** 1:1 mapping:
```
Inside local  (private)  →  Inside global (public)
192.168.10.10            → 203.0.113.10
```

---
# 🛠 4) Allow Only Web Traffic (Optional but Recommended)
Static NAT exposes a private server to the Internet. Use an ACL to limit what can reach it.

```bash
configure terminal
access-list 100 permit tcp any host 203.0.113.10 eq 80
access-list 100 permit tcp any host 203.0.113.10 eq 443
access-list 100 deny ip any any
!
interface gig0/1
 ip access-group 100 in
end
```

This filters inbound traffic on the WAN interface.

---
# 🧪 5) Verification Commands
### ✔ Check NAT Translations
```bash
show ip nat translations
```
Expected example:
```
192.168.10.10  <-> 203.0.113.10   static
```

### ✔ Check Interface NAT Roles
```bash
show ip nat statistics
show ip interface gig0/0
show ip interface gig0/1
```
Look for:
```
IP nat inside
IP nat outside
```

### ✔ Test Reachability (Lab)
From external host:
```bash
ping 203.0.113.10
curl http://203.0.113.10
```

---
# 🔍 6) Optional Add‑Ons
### ✔ Static NAT with Specific Ports (Port Forwarding)
Forward only HTTP to internal server:
```bash
ip nat inside source static tcp 192.168.10.10 80 203.0.113.10 80
```

### ✔ Reserve Additional Public IPs for Future Servers
```bash
ip nat inside source static 192.168.10.20 203.0.113.11
ip nat inside source static 192.168.10.30 203.0.113.12
```

---
# 🏁 7) Final Summary
This configuration file includes:
- Inside/outside interface setup
- Static 1:1 NAT mapping
- Optional inbound ACL protection
- Verification & troubleshooting commands
- Port-forwarding example

This completes **Day 28 – Static NAT – Configuration.md**.
