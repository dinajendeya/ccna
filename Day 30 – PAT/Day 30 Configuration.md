# Day 30 – PAT (Port Address Translation) – Configuration.md
*CCNA configuration for PAT (NAT Overload). This file focuses ONLY on PAT— the most widely used NAT method in the world.*

---
# 📘 1) Scenario Overview
BlueWave University wants **ALL internal hosts** to access the Internet using **ONE** public IP.

Inside LAN: `192.168.20.0/24` on Gig0/0  
Outside/WAN: `203.0.113.1` on Gig0/1  
Public IP used for PAT: **the Gig0/1 interface IP**  

Goal:
- Mark the inside and outside interfaces
- Match inside traffic using an ACL
- Enable PAT using the `overload` keyword
- Verify translations

PAT = Many private → One public using port numbers

---
# 🛠 2) Configure Inside & Outside Interfaces
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
# 🛠 3) ACL to Match Inside Addresses
Select which devices are allowed to use NAT.
```bash
configure terminal
access-list 1 permit 192.168.20.0 0.0.0.255
end
```

---
# 🛠 4) Enable PAT (Core Command)
Use the WAN interface IP for all translations.
```bash
configure terminal
ip nat inside source list 1 interface gig0/1 overload
end
```

This enables PAT:
- Many inside private hosts
- One outside global IP (203.0.113.1)
- Router uses port numbers to track sessions

---
# 🧪 5) Verification Commands
### ✔ NAT Translation Table
```bash
show ip nat translations
```
Expected:
```
Pro Inside Local     Inside Global       Outside Local      Outside Global
--- 192.168.20.10    203.0.113.1:40001   8.8.8.8:80         8.8.8.8:80
```

### ✔ NAT Statistics
```bash
show ip nat statistics
```
Shows number of active translations and hits.

### ✔ Check Interface Roles
```bash
show ip interface gig0/0
show ip interface gig0/1
```
Expected:
```
IP nat inside
IP nat outside
```

---
# 🛠 6) Common Add‑Ons
### A) PAT with Pool Failover (Optional)
If the pool is exhausted, PAT will be used as backup.
```bash
ip nat inside source list 1 pool MYPOOL overload
```

### B) Allow Specific Traffic Through (Optional Firewall)
Combine ACLs to restrict outbound services.
```bash
access-list 100 deny tcp any any eq 23
access-list 100 permit ip any any
```

---
# 🔍 7) Troubleshooting Tips
- **No NAT entries?** → ACL mismatch or missing route.
- **Users can’t browse?** → Check default route:
```bash
ip route 0.0.0.0 0.0.0.0 <ISP-NEXT-HOP>
```
- **PAT overload not working?** → Ensure the interface has a real IP.
- **Return traffic fails?** → ISP must route the public IP back to you.

---
# 🏁 8) Final Summary
This file includes:
- Inside/outside interface config
- ACL match list
- PAT overload command
- Verification & troubleshooting
- Optional NAT enhancements

This completes **Day 30 – PAT – Configuration.md**.
PAT is the most widely used NAT form, enabling scalable Internet access using a single public IP.
