
# Day 26 – Standard ACLs  

> **Note:**  
> This README.md focuses only on explaining **Standard Access Control Lists** (ACLs).  
> All configuration commands will be provided separately in:  
> **Day 26 – Standard ACLs – Configuration.md**

---

# 🧠 1. The Story: Why Do We Need ACLs?

Imagine you’re managing a school network.  
All students and staff are on the network:

- Teachers need access to everything  
- Students should not access administrative servers  
- Guest devices should not access printers  
- Some devices should not access the router itself  

Without traffic filtering, **everyone can talk to everyone**, which creates:

- Security risks  
- Network misuse  
- Uncontrolled access  

This is where ACLs come in.

➡️ **ACLs are like security guards on the router interface.**  
They inspect packets and decide:

✔ Allow this  
❌ Deny this  

ACLs protect networks by filtering traffic based on rules.

---

# 🔐 2. What Is a Standard ACL?

A **Standard ACL** filters traffic **only by Source IP Address**.

It does NOT check:

- Destination  
- Port  
- Protocol  
- Application  
- Service type  

Standard ACLs answer only one question:

> “Who is the sender?”  
Not:  
> “Where are they going?”  
> “Which service do they want?”

Because they filter based only on the **source IP**, they are considered simple but limited.

---

# 🧩 3. Standard ACL Number Ranges

Standard ACLs use the following numbers:

| ACL Type | Range |
|----------|---------|
| Standard Numbered | **1–99** |
| Expanded Standard | **1300–1999** |
| Standard Named | Custom name |

Examples:

```

access-list 10 permit 192.168.10.0 0.0.0.255
access-list 35 deny 10.1.1.5
ip access-list standard BLOCK\_STUDENTS

```

---

# 🎓 4. Where to Place Standard ACLs?

**Rule of thumb:**

### ✔ Place Standard ACLs **as close to the destination as possible.**

Why?

Because standard ACLs filter ONLY by source IP.  
If placed near the source, they might block traffic intended for multiple destinations accidentally.

Example:

If you block “Student network” too early, you might also block access to harmless resources.

So we apply Standard ACLs **near the destination** to avoid unintended blocking.

---

# 📡 5. How Standard ACLs Work

A Standard ACL runs top‑to‑bottom:

1. Packet arrives  
2. ACL checks rule #1  
3. If matched → stop reading  
4. If not matched → move to next rule  
5. If no rules match → packet is denied (implicit deny)

### ✔ ACLs are processed in order  
### ✔ First match wins  
### ✔ Last rule is always:  
```

deny any

```

Even if you don’t write it — it’s always there.

---

# 🧠 6. Understanding Wildcard Masks (The Trickiest Part)

Standard ACLs use **wildcard masks**, not subnet masks.

Wildcard mask examples:

| Subnet | Wildcard |
|---------|-----------|
| 255.255.255.0 | 0.0.0.255 |
| 255.255.0.0 | 0.0.255.255 |
| 255.0.0.0 | 0.255.255.255 |
| Host only | 0.0.0.0 |
| Any IP | 255.255.255.255 |

Wildcard Mask Logic:

- **0 = Must Match Exactly**  
- **1 = Ignore This Bit**

Example:


192.168.1.0 0.0.0.255

    Means:  
    Allow **192.168.1.x** (any host in that subnet)

    ---

# 📝 7. Uses of Standard ACLs

### ✔ Blocking one host  
Example: Block 10.1.1.5 from accessing router services.

### ✔ Allowing only specific trusted devices  
Example: Allow only two admin PCs to Telnet or SSH to the router.

### ✔ Protecting router control plane  
E.g., restricting access to:

- VTY lines (SSH/Telnet)
- SNMP
- NTP

### ✔ Limiting traffic from specific networks  
Basic segmentation where destination does not matter.

---

# 🎯 8. Standard ACL Examples (Concept Only)

### Block one IP address:
```
deny 10.10.10.5
```

### Block an entire network:
```
deny 192.168.20.0 0.0.0.255
```

### Allow a specific host:
```
permit 10.1.1.100
```

### Allow a subnet:
```
permit 172.16.5.0 0.0.0.255
```

### Allow everyone else:
```
permit any

```

(Remember — if you don’t permit “any”, all traffic is denied.)

---

# 🚨 9. Standard ACL Limitations

Standard ACLs cannot:

- Filter by destination  
- Filter by port (e.g., HTTP, FTP, SSH)  
- Block specific services  
- Filter by protocol type  
- Identify applications  
- Match TCP/UDP flags  

For those features, you need:

➡️ **Extended ACLs (Day 27)**

---

# 🧪 10. Real‑World Example (BlueWave University)

BlueWave wants to:

- Allow admin PCs (10.1.1.10 and 10.1.1.11) to access the router  
- Block all student devices from accessing the router  
- Still allow students to use the internet normally  

A Standard ACL allows filtering ONLY based on **source IP**, perfect for controlling router access.

Admins create a **Standard ACL** applied to the **VTY lines** to protect remote access.

---

# 🔄 11. Relationship With Next Day

- **Day 26 (today)** → Standard ACLs (source‑based filtering)  
- **Day 27** → Extended ACLs (source + destination + port + protocol)  
- **Day 28** → Static NAT (ACLs often used for NAT rules)  

This day provides the foundation needed before learning more advanced filtering.

---

# 📝 12. Reminder

This README file contains **no configurations**.  
All real configurations are in:

➡ **Day 26 Configuration.md**

