# Day 25 – First Hop Redundancy Protocols (HSRP / VRRP / GLBP)
> **Important Note:**  
> Cisco **HSRP**, **VRRP**, and **GLBP** are *not supported in Packet Tracer*.  
> Therefore, **no configuration file** is provided for Day 25, 
> but this README.md explains all concepts clearly so you can understand them for CCNA exams and apply them later on real equipment or GNS3/EVE‑NG.

---

# 🧠 1. The Story: Why Do We Need First Hop Redundancy?

Imagine a university campus where:

- Students connect to WiFi  
- Staff connect to LAN  
- All devices use a **default gateway** (router) to reach the internet  

But what happens if the router acting as the default gateway **fails**?

### ❌ Without redundancy:
- No internet  
- No access to servers  
- No communication between buildings  
- Campus network goes down instantly  

This is a single point of failure.

Engineers needed a way to:

- Have **multiple routers** act together  
- Provide **one virtual gateway IP**  
- Allow **automatic failover** if the active router dies  
- Make the entire network more reliable  

This is why First Hop Redundancy Protocols (FHRPs) were created.

➡️ These protocols protect the **default gateway**, which is the *first hop* in a device’s path to the outside network.

---

# 🧩 2. What Are First Hop Redundancy Protocols?

**FHRPs allow multiple routers to share one virtual IP address** that hosts use as their default gateway.

If the active router fails:

- Another router takes over  
- Network continues running  
- Users don’t notice anything  

There are three main FHRPs:

### ✔ HSRP – Hot Standby Router Protocol (Cisco proprietary)  
### ✔ VRRP – Virtual Router Redundancy Protocol (Open standard)  
### ✔ GLBP – Gateway Load Balancing Protocol (Cisco proprietary)

Let’s break them down.

---

# 🔥 3. HSRP (Hot Standby Router Protocol)

**Cisco proprietary**  
Creates:

- **Active router** → handles traffic  
- **Standby router** → monitors active  
- **Virtual IP + Virtual MAC** → used by clients  

If the active router dies:

➡ Standby becomes active in **milliseconds**

### HSRP Priority
- Higher number = higher chance to be active  
- Default = 100  
- Preemption can be used to force higher‑priority router to retake role  

### Key Points:
- Only **one router forwards** at a time  
- Uses multicast **224.0.0.2**  
- Supports multiple HSRP groups  

---

# 🟥 4. VRRP (Virtual Router Redundancy Protocol)

**Open standard** (supported on Cisco, Juniper, Huawei, etc.)

Similar to HSRP but:

- **Master** instead of Active  
- **Backup** instead of Standby  
- Priority range: **1–254** (255 reserved for owner)  

### Key Points:
- Default gateway IP can be the **real IP** of the Master  
- Master preempts by default  
- Uses multicast **224.0.0.18**  
- Faster failover than HSRP by default  

VRRP is common in mixed‑vendor environments.

---

# 🟩 5. GLBP (Gateway Load Balancing Protocol)

Cisco proprietary  
Unlike HSRP/VRRP, **multiple routers can forward traffic simultaneously**.

GLBP roles:

- **AVG (Active Virtual Gateway)**  
  - Distributes virtual MAC addresses  
- **AVFs (Active Virtual Forwarders)**  
  - Actually forward traffic  
  - Allow true load‑balancing  

### Why GLBP is powerful:
- Load balancing across routers  
- Redundancy included by design  
- Better for high‑traffic environments  

Three load‑balancing methods:

- Round‑Robin  
- Weighted  
- Host‑dependent  

---

# 🏫 6. High‑School Analogy

Imagine three security guards responsible for opening the school gate:

### HSRP:
- One active guard  
- One standby guard  
- Standby takes over only if active collapses  

### VRRP:
- Same idea, but standardized so any brand can use it  

### GLBP:
- All guards can open the gate  
- One guard manages who opens for who (AVG)  

GLBP spreads the work, while HSRP/VRRP only use one guard at a time.

---

# 🧪 7. Real‑World Example (BlueWave University)

BlueWave wants to protect its internet gateway:

Routers:
- R1 → 10.1.1.1  
- R2 → 10.1.1.2  

Virtual gateway:
- 10.1.1.254  

All PCs use **10.1.1.254** as the default gateway.

If R1 fails:

- With HSRP → R2 becomes Active  
- With VRRP → R2 becomes Master  
- With GLBP → R2 takes over forwarding for its virtual MAC group  

Users see **zero downtime**.

---

# 📡 8. How Devices Use FHRP

Devices (PCs, laptops, servers):

- Do NOT know about multiple routers  
- Only know a single default gateway IP  
- Routers negotiate among themselves which one should forward  
- Failover is transparent  

No user reconfiguration needed.

---

# 🔍 9. Comparison Table (Simplified)

| Feature | HSRP | VRRP | GLBP |
|--------|-------|-------|--------|
| Standard | Cisco | Open | Cisco |
| Redundancy | Yes | Yes | Yes |
| Load Balancing | No | Limited | **Yes** |
| Virtual Router | Yes | Yes | Yes |
| Preemption | Off by default | On by default | On |
| Multicast | 224.0.0.2 | 224.0.0.18 | 224.0.0.102 |

---

# 🧠 10. Why Packet Tracer Does Not Support FHRPs

Packet Tracer simulates:

- CCNA basic routing  
- VLANs  
- OSPF / EIGRP  
- ACLs  
- NAT / DHCP  

But **it does NOT simulate FHRP protocols** because:

- Requires advanced L3 switch features  
- Requires simulation of virtual MAC swapping  
- Requires multi-router state synchronization  
- Needs higher hardware abstraction  

- **GNS3** supports HSRP/VRRP/GLBP:

That’s why this day includes concepts only — **no configuration file**.

---

# 🔄 11. Relationship With Previous Days

- Day 21 → Dynamic Routing  
- Day 22 → EIGRP  
- Day 23 → OSPF (Point‑to‑Point)  
- Day 24 → OSPF (Broadcast, DR/BDR)  
- **Day 25 → FHRPs for gateway redundancy**  

In the next days, we’ll continue with:  
- ACLs  
- NAT  
- IPv6  
- DHCP  
- Security features  
- SNMP / Syslog  

Each building on the previous foundation.

---

# 📝 12. Reminder

Packet Tracer does **NOT** support:

- HSRP  
- VRRP  
- GLBP  

So there will be:

❌ No `Day 25 Configuration.md`  
✔ Only README concepts

