# Day 21 – Dynamic Routing
*A Clear, Beginner‑Friendly Story + Full CCNA‑Level Explanation*  
> **Note:**  
> This README.md contains **only conceptual explanation**.  
> All router configuration commands will be included in:  
> **Day 21 Configuration.md**

---

# 🧠 1. The Story: Why Do We Need Dynamic Routing?

Imagine a university campus with 10 different buildings.

Each building has:

- its own network  
- its own router  
- its own VLANs  

Now imagine this:

- One of the fiber links between buildings goes down  
- A new department is added with a new network  
- A link becomes congested  
- A router fails  

If you used **static routing** (Day 09), you'd have to:

- manually update routes on every router  
- manually remove invalid paths  
- manually add new networks  
- manually update backup routes

For a campus with dozens of routers, **this becomes a nightmare**.

Network engineers needed a smarter method:

➡️ **Routers should exchange routes automatically.**  
➡️ **Routers should detect failures and re-route instantly.**  
➡️ **Routers should figure out the best path by themselves.**

This is how **dynamic routing** was born.

---

# 📡 2. What Is Dynamic Routing?

**Dynamic routing** means routers automatically:

- discover networks  
- exchange route information  
- update routing tables  
- detect failures  
- choose the best path  
- reroute traffic quickly  

This is done using **routing protocols**.

Dynamic routing = routers "talking" to each other like GPS systems:

> “Hey, I know how to reach this network.”  
> “This link is down, use another route.”  
> “The path changed, here’s the new route.”

---

# 🧩 3. Static Routing vs Dynamic Routing

### ✔ Static Routing  
- Manually configured  
- No overhead  
- Secure & predictable  
- Good for small networks  

### ✔ Dynamic Routing  
- Auto-learn networks  
- Auto-update when topology changes  
- Scales easily  
- Best for medium–large networks  

| Feature | Static | Dynamic |
|---------|--------|---------|
| Manual configuration | Yes | No |
| Auto update | No | Yes |
| Failure detection | No | Yes |
| Scalability | Low | Very High |
| Used in? | Small LANs | Campus & enterprise |

Dynamic routing saves hours of work and prevents outages.

---

# 🔌 4. Types of Dynamic Routing Protocols

Dynamic routing protocols fall into two main categories:

---

## 🟦 A. **Interior Gateway Protocols (IGPs)**  
Used **inside** an organization (LAN/WAN/campus).

Includes:

### ✔ **RIP**  
- Oldest  
- Distance vector  
- Uses hop count  
- Max 15 hops  
- Slow convergence  
- Simple but outdated  

### ✔ **EIGRP**  
- Cisco proprietary  
- Hybrid protocol  
- Fast convergence  
- Efficient path calculation  

### ✔ **OSPF**  
- Industry standard  
- Link-state protocol  
- Hierarchical design  
- Fast and scalable  
- Used in large enterprise networks  

---

## 🟥 B. **Exterior Gateway Protocols (EGPs)**  
Used **between** organizations—like the Internet.

### ✔ **BGP** (Border Gateway Protocol)  
- The protocol that *runs the global Internet*  
- Path vector  
- Used by ISPs and large enterprises  

(BGP is NOT part of CCNA depth, but you should know what it is.)

---

# 📍 5. How Dynamic Routing Works (The Essentials)

Dynamic routing protocols exchange special messages that contain:

- known networks  
- interface states  
- link costs  
- neighbor information  

Routers use these to build a **routing table**.

Routing table columns:

| Destination | Next Hop | Outbound Interface | Metric |
|-------------|-----------|----------------------|---------|

The protocol chooses the **best path** based on a "metric."

Examples:

- RIP → hop count  
- OSPF → cost (bandwidth-based)  
- EIGRP → composite metric  

Routing is recalculated automatically when:

- a link fails  
- a new link is added  
- a router joins the network  
- network topology changes  

---

# 🛰 6. Convergence – The Most Important Concept

**Convergence** = when all routers agree on the network topology.

Fast convergence = stable network.  
Slow convergence = loops, outages, instability.

Ranking by speed:

| Protocol | Speed |
|----------|--------|
| RIP | ❌ Slow |
| EIGRP | ⚡ Fast |
| OSPF | ⚡ Fast |
| RSTP (Layer 2) | Not a routing protocol, but extremely fast |

Dynamic routing protocols aim to reach convergence quickly so traffic flows normally.

---

# 🧠 7. Administrative Distance (AD)

**Administrative Distance** = how trustworthy a route source is.

Lower = better.

Common AD values:

| Source | AD |
|--------|-----|
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

The router chooses routes from the protocol with the **lowest AD**.

Example:

If EIGRP and RIP both know a path to 10.10.10.0/24 →  
The router uses EIGRP (AD 90) instead of RIP (AD 120).

---

# 🏫 8. High‑School Analogy

Think of routers as students in a school:

- Each student discovers new hallways (networks)
- They share information with friends (neighbors)
- They pick fastest route to classrooms (best path)
- If a hallway is blocked, they tell everyone
- Everyone updates their mental map instantly

This creates a network of students who "learn" the best paths in real time.

---

# 🧪 9. Real‑World Example at BlueWave University

BlueWave has:

- 3 buildings  
- 12 VLANs  
- 8 routers  
- Multiple fiber uplinks  

Using **static routes** would require hundreds of manual commands.

Using dynamic routing:

- OSPF automatically discovers new networks  
- EIGRP quickly reroutes if a fiber link fails  
- All routers instantly converge  
- Students (users) never notice downtime  

Dynamic routing = **self‑healing**, **self‑updating**, and **automated**.

---

# 🔄 10. Relationship With Other Days

### Previous days
- Day 09 → Routing Fundamentals  
- Day 14 → Inter‑VLAN Routing  
- Day 20 → EtherChannel increases bandwidth for routing  

### Day 21 (Today)
Dynamic Routing foundation:  
- Why dynamic routing is needed  
- Types of protocols  
- Convergence, metrics, AD  

### Future days
- **Day 22 → EIGRP**  
- **Day 23 → OSPF (Part 1)**  
- **Day 24 → OSPF (Part 2)**  
Each protocol will have its own README + configuration.

---

# 📝 11. Reminder

This README includes **only explanations**.  
All configurations (RIP, EIGRP, OSPF examples) are in:

➡ **Day 21 Configuration.md**

