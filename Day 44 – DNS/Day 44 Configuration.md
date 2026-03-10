# Day 44 – DNS – configuration.md
*Basic DNS configuration aligned with CCNA scope.*
---
# 📘 1. Scenario Overview
Goal: Create a basic DNS server in Packet Tracer or on a simple Cisco IOS device for testing name-to-IP resolution.

We will configure:
- A local DNS entry for a website (e.g., **www.school.com**)
- A router/switch pointing to that DNS server
- Basic DNS lookup tests

> ⚠ **Packet Tracer Limitation:** Only the "Server" device provides DNS features. Cisco IOS DNS server on routers is limited and not fully realistic.

---
# 🛠 2. Packet Tracer – DNS Server Setup
Using the **Generic Server** device:

### Step 1 — Add a Server
Place a **Server** → connect it to the LAN.

### Step 2 — Assign an IP Address
```
Server IP: 192.168.10.10
Subnet:    255.255.255.0
Gateway:   192.168.10.1
``` 

### Step 3 — Enable DNS Service
GUI navigation:
```
Services > DNS > On
```

### Step 4 — Add DNS Records
Add records like:
| Name | Type | Address |
|------|------|---------|
| www.school.com | A | 192.168.10.50 |
| files.school.com | A | 192.168.10.60 |

Click **Add**.

---
# 🛠 3. Router/Switch – Point to DNS Server
End devices must know the DNS server IP.

### Option A — Using DHCP (Recommended)
Set DNS in your DHCP pool:
```bash
configure terminal
ip dhcp pool LAN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.10
end
```

### Option B — Manually on a PC
On a PC:
```
Desktop > IP Configuration > DNS Server: 192.168.10.10
```

---
# 🧪 4. Testing DNS Resolution
### Test 1 — Ping a name
```
ping www.school.com
```
Expected:
- Packet Tracer resolves the name
- Pings the mapped IP (192.168.10.50)

### Test 2 — From Command Prompt on PC
```
nslookup www.school.com
```
(Expected only if Packet Tracer version supports nslookup.)

---
# 🧪 5. Basic IOS DNS Resolver (Optional)
Routers can use DNS for name resolution.

```bash
configure terminal
ip domain-lookup
ip name-server 192.168.10.10
end
```

### Test on IOS Router
```
ping www.school.com
```
Router will query the DNS server.

---
# 📝 6. Expected Behavior in Labs
- PCs resolve names to correct IPs.
- Router resolves hostnames if name-server is configured.
- DNS server responds only to records you create.
- Packet Tracer provides simple DNS only (no zones, MX, CNAME, etc.).

---
# ⚠ Packet Tracer Limitations
- No advanced DNS features (CNAME, MX, advanced TTL, caching)
- No authoritative DNS behavior
- No DNSSEC or reverse lookup zones
- Only basic **A-record** support

---
# 🏁 Final Summary
This configuration file provides:
- Packet Tracer DNS setup
- Basic DNS records
- DHCP-based DNS delivery
- Router DNS lookup configuration (optional)
- Testing steps and expected behavior
