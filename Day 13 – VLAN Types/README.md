# Day 13 – VLAN Types

## 🧠 1. The Story: Why Do We Have Different VLAN Types?

Imagine a large school building again:

- Students  
- Teachers  
- Cameras  
- Administration  
- Maintenance staff  

Everyone shares the same physical building but has **different roles, responsibilities, and access levels**.

If all traffic (WiFi, cameras, computers, guests, etc.) were mixed into one network, it would be:

- slow  
- insecure  
- chaotic  

Engineers solved this by creating **different types of VLANs**, each serving a special purpose — like giving each group its own hallway and security rules.

This is where we get:

- **Default VLAN**  
- **Data VLAN**  
- **Management VLAN**  
- **Native VLAN**  

Each one plays a unique role in keeping networks safe, fast, and organized.

---

# 🧩 2. Default VLAN  
**The "factory settings" VLAN of a switch**

When you take a brand‑new switch out of the box:

- All ports belong to **VLAN 1**  
- CDP, LLDP, STP, PAgP, etc., run on VLAN 1  
- It cannot be deleted  

### Why VLAN 1 exists:
It gives the switch a “safe starting point” before you configure anything.  
Think of it as the “default classroom” before you assign students to real classes.

### Best practice:
✔ DO NOT use VLAN 1 for users  
✔ DO NOT send management traffic on VLAN 1  
✔ DO NOT leave trunk native VLAN = 1 (security risk)

---

# 📊 3. Data VLAN  
**The VLAN used by end devices (PCs, phones, printers, etc.)**

This is the VLAN where *normal users* live.

Examples:

- VLAN 10 → IT  
- VLAN 20 → HR  
- VLAN 30 → Students  

Data VLANs contain:

- User applications  
- File sharing  
- Browsing  
- Employee laptops  
- Desktop PCs  

### Why Data VLANs exist:
They organize departments into separate broadcast domains so:

- HR traffic stays private  
- IT traffic stays fast  
- Student traffic cannot reach sensitive resources  

---

# 🔐 4. Management VLAN  
**The VLAN used to remotely manage the switch**

This VLAN is CRITICAL.

When you connect to a switch using:

- SSH  
- Telnet (not recommended)  
- HTTPS (web UI)  
- SNMP  
- Syslog  
- NetFlow  

…the traffic goes through the **Management VLAN**.

A switch has an **SVI (Switch Virtual Interface)** created inside the management VLAN.

Example:  
If the management VLAN is 99, the switch interface is:
Interface vlan 99
ip address 192.168.99.10 255.255.255.0


### Why Management VLANs exist:
- To separate switch administration from user data  
- To protect the switch from unauthorized access  
- To support remote configuration  
- To keep the control plane secure and isolated  

### Very important:
✔ Management VLAN must NOT be VLAN 1  
✔ Access to it should be restricted with ACLs  
✔ Only admins should use it  

---

# 🟧 5. Native VLAN  
**The VLAN that carries untagged traffic on 802.1Q trunk links**

When two switches connect using a **trunk**, frames normally include VLAN tags.

But sometimes a frame arrives **without any tag**.

The switch needs a place to put this untagged frame.

That place = the **Native VLAN**.

Default native VLAN = **VLAN 1**  
Best practice = change it (example VLAN 99 or 999)

### Why Native VLAN exists:
- Backward compatibility with old devices  
- Support for voice phones  
- Support for untagged control protocols  

### Security warning:
If you leave the native VLAN as VLAN 1, attackers can use VLAN hopping techniques.

---

# 🧠 6. Summary Comparison Table

| VLAN Type | Purpose | Best Practice | Notes |
|----------|----------|----------------|-------|
| **Default VLAN (1)** | Factory setup, protocols run here | Do NOT use for users or management | Cannot be deleted |
| **Data VLAN** | User devices & normal traffic | One VLAN per department | Main VLAN type used daily |
| **Management VLAN** | Remote switch management (SSH, SNMP, HTTPS) | Must NOT be VLAN 1 | Use ACLs to secure |
| **Native VLAN** | Handles untagged frames on trunk ports | Change from VLAN 1 | Only 1 native VLAN per trunk |

---

# 🏫 7. Simple Real‑World Example 

A university has one switch on each floor.

They create:

- VLAN 10 → IT staff  
- VLAN 20 → Teachers  
- VLAN 30 → Students  
- VLAN 40 → Security cameras  
- VLAN 99 → Management VLAN  
- VLAN 999 → Native VLAN on trunks  

What happens:

- Students cannot see cameras  
- Teachers cannot see IT laptops  
- Switches are remotely managed on VLAN 99  
- Trunks between floors use native VLAN 999  
- Default VLAN 1 is unused for safety  

This is clean, secure, professional design.

---

# 📝 8. Reminder

This is the **README.md** explanation only.  
All VLAN Types configuration commands will be available in:

➡ **Day 13 – VLAN Types → configuration.md**
