
# Day 27 – Extended ACLs  

> **Note:**  
> This README.md contains *only conceptual explanation*.  
> All configuration commands will be included in:  
> **Day 27 – Extended ACLs – Configuration.md**

---

# 🧠 1. The Story: Why Do We Need Extended ACLs?

In **Day 26**, you learned about *Standard ACLs*, which filter traffic by **source IP only**.

But imagine you're running the network at a university:

- Students can access *web servers*, but they shouldn't use *SSH*  
- Admins need *SSH* but not *FTP*  
- Guests should only access the *internet*, not internal servers  
- Certain subnets need *HTTP blocked* but *HTTPS allowed*  

Can Standard ACLs handle this?

### ❌ No — because Standard ACLs only check the sender’s IP.

We need something more powerful.

➡️ **Extended ACLs** were created to filter traffic using multiple criteria:

- Source IP  
- Destination IP  
- Protocol (TCP, UDP, ICMP, etc.)  
- Port numbers (80, 443, 22, 21, etc.)  
- Services (HTTP, DNS, FTP, etc.)  

Extended ACLs allow real security, precision, and control.

---

# 🔍 2. What Is an Extended ACL?

Extended ACLs answer ALL of these questions:

### ✔ Who is sending the traffic? (source IP)  
### ✔ Where is it going? (destination IP)  
### ✔ What service is being used? (port/protocol)  
### ✔ Should the packet be allowed or denied?

This makes Extended ACLs powerful and flexible.

---

# 🔢 3. Extended ACL Number Ranges

| ACL Type | Range |
|----------|--------|
| **Extended Numbered ACLs** | **100–199** |
| | **2000–2699** |
| **Extended Named ACLs** | Custom name |

Examples:

```

access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 80
ip access-list extended BLOCK\_SSH

```

---

# 🎯 4. Where to Place Extended ACLs?

**Rule of thumb:**

### ✔ Place Extended ACLs *as close to the source as possible.*

Because if you block traffic early, it never wastes bandwidth along the path.

Example:

If you want to stop students from reaching a server:

→ Block them at their VLAN interface, not at the server interface.

---

# 🧠 5. The Extended ACL Structure (VERY Important)

Extended ACL rule format:

```

permit|deny protocol source\_IP wildcard destination\_IP wildcard \[operator port]

```

Example:

```

deny tcp 192.168.50.0 0.0.0.255 10.1.1.10 0.0.0.0 eq 22

```

This line means:

> Deny TCP traffic  
> from students (192.168.50.x)  
> going to admin server (10.1.1.10)  
> on port 22 (SSH)

---

# 🧩 6. Protocols Supported by Extended ACLs

Common choices:

- **ip** → all Layer‑3 protocols  
- **tcp** → web, ssh, ftp, telnet, etc.  
- **udp** → dns, dhcp, snmp, etc.  
- **icmp** → ping, traceroute  
---

# 🔥 7. Port Numbers You Must Know for CCNA

| Service | Port | Protocol |
|---------|------|----------|
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| FTP | 20/21 | TCP |
| DNS | 53 | TCP/UDP |
| DHCP | 67/68 | UDP |
| SNMP | 161 | UDP |

Extended ACLs can filter based on these *exact* services.

---

# 📘 8. Examples of What Extended ACLs Can Do

### ✔ Block SSH but Allow Web Traffic
```

deny tcp any any eq 22
permit tcp any any eq 80

```

### ✔ Allow only one subnet to use DNS
```

permit udp 10.1.1.0 0.0.0.255 any eq 53
deny udp any any eq 53

```

### ✔ Block ping (ICMP)
```

deny icmp any any echo

```

### ✔ Allow HTTPS to a specific server only
```

permit tcp any host 10.1.1.50 eq 443
deny tcp any host 10.1.1.50

```

---

# 📍 9. Real‑World Example (BlueWave University)

BlueWave has:

- Students in **192.168.50.0/24**  
- Admin servers **10.1.1.10–20**  
- Guest WiFi in **172.16.0.0/16**

Security policy:

- Students → can only use HTTP/HTTPS to internet  
- Students → *cannot* access admin servers  
- Guests → internet only (no internal networks)  
- Admins → can access everything  

Extended ACLs allow this with fine detail.

---

# 🧪 10. Understanding “any”, “host”, and wildcards

### **any**
```

0.0.0.0 255.255.255.255

```

### **host 10.1.1.10**
```

10.1.1.10 0.0.0.0

```

### **subnet example**
```

192.168.50.0 0.0.0.255

```

Extended ACLs require *exact* wildcard masks.

---

# ⚠ 11. Implicit Deny Still Applies!

All ACLs end with:

```

deny ip any any

```

If you don't add a final `permit ip any any`, everything except what you permit will be blocked.

---

# 🔄 12. Relationship with Next Days

| Day | Topic |
|-----|--------|
| **Day 26** | Standard ACLs |
| **Day 27** | Extended ACLs (today) |
| **Day 28** | Static NAT (uses ACLs!) |
| **Day 29** | Dynamic NAT |
| **Day 30** | PAT |

Extended ACLs are essential for all NAT topics.

---

# 📝 13. Reminder

This README does **not** include configuration commands.  
They will be provided separately in:

➡ **Day 27 – Extended ACLs – Configuration.md**
