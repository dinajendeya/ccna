
# Day 24 – OSPF (Part 2)  
*OSPF Broadcast Networks, DR/BDR Elections, Multicast, Loopback Behavior, Router ID Priority, and OSPF Interface Priority*
  
> **Note:**  
> - This README.md contains **only conceptual explanation**.  
> - Configuration commands will be provided in:  
>   **Day 24 – OSPF (Part 2) – Configuration.md**  
> - Day 23 covered OSPF fundamentals + Point‑to‑Point networks.  
> - Day 24 (this file) focuses on **multi‑access Ethernet OSPF behavior**.

---

# 🧠 1. Why OSPF Behaves Differently on Multi‑Access Networks

In **point‑to‑point** OSPF (Day 23), only **two routers** exchange LSAs.  
But in **Ethernet LANs**, there could be:

- 5 routers  
- 10 routers  
- 50 routers  

If every router sent LSAs to every other router, the network would explode with traffic.

Engineers needed a way to:

- Reduce LSA flooding  
- Prevent chaos in multi‑router Ethernet segments  
- Optimize OSPF operations

➡️ The solution: **DR and BDR elections**.

---

# 🌐 2. OSPF Network Types (Today: Broadcast Networks)

On Ethernet networks, OSPF uses the **Broadcast network type**:

- Uses **multicast** packets  
- Elects a **DR** (Designated Router)  
- Elects a **BDR** (Backup Designated Router)  
- All other routers become **DROthers**

This reduces LSAs flooding and centralizes communication.

---

# 🔥 3. What Are DR and BDR?

### ✔ DR – Designated Router  
The router responsible for:

- Flooding LSAs to all routers on the LAN  
- Maintaining adjacency only with BDR + DROthers  
- Acting as the focal point for database exchange

### ✔ BDR – Backup Designated Router  
Stands by.  
If the DR fails → BDR becomes DR instantly.

### ✔ DROthers  
All other routers on that broadcast network.

---

# 🧩 4. Why DR/BDR Are Needed

Imagine 10 routers on the same switch port.

### Without DR/BDR  
Each router must form adjacency with 9 routers → 45 OSPF relationships  
Complex & heavy traffic.

### With DR/BDR  
Each router forms adjacency only with DR & BDR → **2 neighbors only**  
Much simpler & efficient.

---

# 📝 5. DR/BDR Election Rules

OSPF elects DR/BDR based on:

### 1. **Highest OSPF Priority (0–255)**  
Default = 1  
Priority 0 = cannot become DR/BDR

### 2. **Highest Router ID (RID)**  
If priority ties → highest RID wins.

### 3. **First come, first served**  
Once elected, DR/BDR stay unless the interface goes down.

This means:

❗ Restarting routers will NOT trigger re-election unless DR/BDR fail.

---

# 🔍 6. OSPF Multicast Addresses

Broadcast networks use two important multicast addresses:

| Multicast | Used By |
|-----------|----------|
| **224.0.0.5** | All OSPF routers |
| **224.0.0.6** | DR/BDR only |

### How they work:

- Regular routers send Hellos to **224.0.0.5**
- DR/BDR listen on **224.0.0.6**
- LSAs destined for all routers → .5
- LSAs intended for DR/BDR → .6

This reduces unnecessary flooding.

---

# 🧠 7. Loopback Interfaces in OSPF

A **loopback interface** is:

- A virtual interface  
- Always up unless manually shut  
- The most stable interface on a router  

OSPF uses loopbacks:

### ✔ For Router ID selection  
Highest loopback IP wins (unless manually set).

### ✔ To create stable injection points  
Often used for:

- Management IP  
- OSPF RID  
- Redistribution points  
- Routing sources  

### OSPF treats loopbacks as *host routes* (/32)

Even if configured as `/24`, OSPF advertises it as `/32` by default.

This can be changed by altering OSPF network type (covered in config file).

---

# 🧮 8. Router ID Priority (VERY Important for CCNA)

OSPF chooses the Router ID using the following priority:

### 1️⃣ Manual RID  
```

router-id x.x.x.x

```

### 2️⃣ Highest Loopback Interface IP  
Loopback ensures RID stability.

### 3️⃣ Highest Physical Interface IP  
Fallback if no loopback exists.

⚠ If RID has been assigned already, OSPF must be reset:  
```

clear ip ospf process

```

---

# 📡 9. OSPF Interface Priority (Influences DR/BDR Election)

OSPF interface priority determines election outcome:

```

ip ospf priority <0-255>

```

- Higher number → higher chance to become DR
- Priority 0 → cannot participate in elections

Example:

- SW1 priority 200 → will almost always become DR  
- SW2 priority 150 → likely BDR  
- SW3 priority 0 → DROther only  

Changing priority requires interface reset or OSPF process reset.

---

# 📘 10. Example Scenario (BlueWave University LAN)

Three routers in VLAN 10:

- R1 (RID 1.1.1.1)  
- R2 (RID 2.2.2.2)  
- R3 (RID 3.3.3.3)  

Priorities:

- R1 → priority 100  
- R2 → priority 1  
- R3 → priority 0  

Election result:

- **DR = R1** (highest priority)  
- **BDR = R2** (second highest priority)  
- **DROther = R3** (priority 0 cannot become DR/BDR)

All routers exchange LSAs via R1 (DR) and R2 (BDR).

---

# 🔄 11. Relationship With Part 1 (Day 23)

| Day | Topic |
|-----|--------|
| **Day 23** | OSPF basics + point‑to‑point networks |
| **Day 24** | Broadcast networks + DR/BDR + multicast + loopbacks + RID selection |
| Day 25 | FHRP introduction (HSRP/VRRP/GLBP) |

Broadcast OSPF completes your understanding before doing advanced multi‑area OSPF in later days.

---

# 📝 12. Reminder

All configurations—covering:

- Loopback networks  
- Network types  
- Priority manipulation  
- DR/BDR tuning  
- Multicast verification  
- RID manual configuration  

…will be provided separately in:

➡ **Day 24 – OSPF (Part 2) – Configuration.md**


