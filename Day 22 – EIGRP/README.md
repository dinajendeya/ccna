
# Day 22 – EIGRP (Enhanced Interior Gateway Routing Protocol)  

> **Note:** This README.md contains *only* conceptual explanation.  
> All router configurations will be provided separately in:  
> **Day 22 – EIGRP – Configuration.md**

---

# 🧠 1. The Story: Why Was EIGRP Created?

In the early days of networking, engineers had two main routing protocol choices:

- **RIP** → simple but slow, limited, and not scalable  
- **OSPF** → fast and scalable, but complex to deploy and maintain  

Cisco wanted something:

- Faster than RIP  
- Simpler than OSPF  
- Smarter, more optimized, and more efficient  
- Designed for medium/large enterprise networks  

So they created:

➡️ **EIGRP (Enhanced Interior Gateway Routing Protocol)**  
A *hybrid* dynamic routing protocol that combines the best of distance‑vector and link‑state designs.

For many years, EIGRP was **Cisco‑proprietary**, making it extremely popular in Cisco‑based enterprise networks.  
Later, Cisco partially published it as an open standard.

EIGRP became known for being:

✔ Fast  
✔ Efficient  
✔ Reliable  
✔ Easy to configure  

---

# 🔍 2. What Is EIGRP?

**EIGRP = an advanced, hybrid dynamic routing protocol designed by Cisco.**

It is considered:

- Classless  
- Supports VLSM  
- Supports IPv4 & IPv6  
- Uses the DUAL algorithm for fast convergence  
- Uses metrics (bandwidth, delay, load, reliability) to choose best path  

EIGRP is used inside LAN/WAN networks and is categorized as an **IGP** (Interior Gateway Protocol).

---

# 🧩 3. EIGRP vs RIP vs OSPF (Simple Comparison)

| Feature | RIP | OSPF | **EIGRP** |
|--------|------|-------|-----------|
| Type | Distance-Vector | Link-State | **Hybrid** |
| Convergence | Very slow | Fast | **Very fast** |
| Metric | Hop count | Cost | **Composite metric** |
| Scalability | Low | High | **High** |
| Ease of config | Very easy | Moderate | **Easy** |
| Proprietary | No | No | **Originally yes (Cisco)** |

EIGRP sits perfectly between RIP’s simplicity and OSPF’s performance.

---

# 📡 4. EIGRP Metrics (How It Chooses Best Path)

EIGRP calculates routes using a **composite metric** based on:

- **Bandwidth** (lowest bandwidth in path)
- **Delay** (sum of all delays)
- (Optional: Load, Reliability, MTU)

Default formula uses **bandwidth + delay** only.

This makes EIGRP more accurate than RIP’s hop count and more flexible than OSPF’s cost.

---

# 🔥 5. The DUAL Algorithm (EIGRP’s Secret Weapon)

**DUAL = Diffusing Update Algorithm**  
This algorithm gives EIGRP its biggest advantages:

### ✔ Extremely fast convergence  
Routers recalculate only affected routes, not the whole table.

### ✔ Loop-free routes  
DUAL guarantees no routing loops.

### ✔ Backup paths (Feasible Successors)  
EIGRP stores “backup” routes in case the main route fails.

This makes EIGRP one of the most stable IGPs.

---

# 🧠 6. EIGRP Terminology (Simple Explanations)

### ✔ Neighbor  
A directly connected router running the same EIGRP settings.

### ✔ Successor  
The primary (best) route to a destination—installed in routing table.

### ✔ Feasible Successor  
A pre‑calculated backup route—ready for instant failover.

### ✔ Reported Distance (RD)  
Distance reported by neighbor.

### ✔ Feasible Distance (FD)  
Total metric from this router to the destination.

These values help DUAL decide the best and backup routes.

---

# 🔌 7. How EIGRP Forms Neighbor Adjacencies

Routers become neighbors if:

- They are in the **same EIGRP autonomous system (AS number)**
- They share the **same K-values** (metric config)
- They are connected on the same network
- They exchange “Hello” packets

Hello packets default timers:

- **5 seconds** → high‑speed links  
- **60 seconds** → low‑speed links  

Once adjacency forms, routers exchange full routing information.

---

# 🧬 8. EIGRP Packet Types

EIGRP uses 5 packet types:

1. **Hello** → find/maintain neighbors  
2. **Update** → share routes  
3. **Query** → ask neighbors for alternate routes  
4. **Reply** → respond to queries  
5. **ACK** → acknowledgments

These allow EIGRP to operate efficiently and intelligently.

---

# 🏗 9. EIGRP Autonomous System (AS Number)

EIGRP uses an **AS number** to group routers.

Example:

```

router eigrp 100

```

Routers must use the same AS number to form adjacency.

---

# 🧪 10. Real‑World Example (BlueWave University)

BlueWave uses EIGRP for its large campus:

- Building A → VLAN 10, 20, 30  
- Building B → VLAN 40, 50  
- Building C → VLAN 60, 70  
- Multiple routers connected via fiber  

Why EIGRP?

- Campus is Cisco‑based  
- Needs fast convergence for VoIP, video lectures  
- DUAL provides instant failover  
- Easy to manage and scale  
- Perfect for multi‑building topologies  

If a fiber link fails:

- EIGRP switches to a Feasible Successor path instantly  
- Students and staff don’t notice anything  

---

# 🛰 11. EIGRP for IPv6 (Future Topic)

EIGRP supports IPv6 with slightly different syntax.

This will be covered later when we reach:

➡ **Day 33 – IPv6 OSPF**  
➡ **Also as an extension within IPv6 routing topics**

---

# 🔄 12. Relationship With Other Days

### Before this:
- Day 21 → Dynamic Routing theory  
- Day 22 (Today) → EIGRP fundamentals  
- Day 23 → (Already planned to cover EIGRP but we’re using this day instead)

### After this:
- **Day 24 – OSPF (Part 1)**  
- **Day 25 – FHRPs (HSRP / VRRP / GLBP)**  

Dynamic routing continues with OSPF next.

---

# 📝 13. Reminder

This file contains conceptual knowledge only.  
All commands are included separately in:

➡ **Day 22 – EIGRP – Configuration.md**

