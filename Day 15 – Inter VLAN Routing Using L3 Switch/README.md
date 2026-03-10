# Day 15 – Inter‑VLAN Routing Using a Layer 3 Switch (SVI Routing)

> **Note:** All configuration commands for this day are placed in the separate  
> **`configuration.md`** file.  
> Day 14 covered *Inter‑VLAN Routing using a Router (Router‑on‑a‑Stick)*.  
> Day 15 focuses on the **modern**, faster, enterprise‑grade method:  
> **Inter‑VLAN Routing using a Layer 3 Switch**.

---

# 🧠 1. The Story: Why Layer 3 Switch Routing Exists

In **Day 14**, we used a single router interface with sub‑interfaces to route between VLANs.  
This is simple, and great for small networks…

…but imagine a university with:

- Thousands of students  
- Hundreds of staff  
- Security cameras  
- Lab networks  
- Guest WiFi  
- Surveillance and IoT devices  

All VLAN traffic would have to pass through **ONE** router interface.  
This creates a massive bottleneck.

Routers route using **software**, which is slower.  
Switches forward frames using **hardware ASICs**, which is extremely fast.

Engineers needed a way to:

- Route **between VLANs at switch speeds**  
- Eliminate the router bottleneck  
- Scale to large enterprise networks  
- Keep the network efficient and stable  

💡 The solution: **Layer 3 Switches performing Inter‑VLAN Routing internally using SVIs.**

---

# 🧩 2. What Is a Layer 3 Switch?

A Layer 3 switch is a device that:

- Acts like a **Layer 2 switch** (VLANs, STP, trunks)  
- Acts like a **router** (IP routing, routing tables, dynamic protocols)  

It combines the best of both worlds:

- Switch-level speed  
- Router-level intelligence  

This makes it ideal for modern campus and enterprise networks.

---

# 🧱 3. What Is an SVI (Switch Virtual Interface)?

An **SVI** is a *virtual* Layer 3 interface created on a switch to represent a VLAN.

Think of it as:

- A “router inside the switch”
- One virtual gateway per VLAN
- No need for physical router connections

Example:
interface vlan 10
ip address 192.168.10.1 255.255.255.0

This SVI becomes the **default gateway** for all devices in VLAN 10.

The switch then routes between VLAN 10, VLAN 20, VLAN 30… all internally.

---

# 🔥 4. The Concept: Inter‑VLAN Routing Using SVIs

### Step 1  
Create VLANs on the switch.

### Step 2  
Create an SVI for each VLAN (like a router interface).

### Step 3  
Enable Layer 3 routing on the switch.

### Step 4  
Switch handles all routing internally — *no external router needed*.

---

# 5. Why SVI Routing Is Faster Than Router-on-a-Stick

### Router-on-a-Stick  
- One physical link  
- Software routing  
- Can bottleneck easily  
- Good only for small networks  

### SVI Routing (L3 Switch)  
- Routes inside switch fabric  
- Hardware‑accelerated  
- No bottlenecks  
- Scales to thousands of users  
- Enterprise standard  

This is why most companies use L3 switches in their distribution layer.

---

# 🏫 6. High‑School Analogy

Imagine a school with 10 classrooms on each floor.

**Day 14 method (Router):**  
You have one elevator that everyone must use to go between floors → slow.

**Day 15 method (L3 Switch with SVIs):**  
The building adds **stairs inside every floor** so people can move instantly → no waiting.

---

# 🧠 7. How Communication Works with SVIs

Example VLAN Layout:

| VLAN | Subnet | Gateway (SVI) |
|------|---------|-----------------|
| 10 | 192.168.10.0/24 | 192.168.10.1 |
| 20 | 192.168.20.0/24 | 192.168.20.1 |
| 30 | 192.168.30.0/24 | 192.168.30.1 |

### Example Communication  
A PC in VLAN 10 wants to talk to a server in VLAN 20:

1. PC sends packet to its gateway → SVI VLAN 10  
2. L3 switch routes it internally  
3. Switch forwards it out of VLAN 20  
4. No router needed  
5. It happens in microseconds thanks to hardware ASICs  

---

# 🧩 8. Requirements for SVI Routing

A switch must:

- Support Layer 3 features (not all do)  
- Have SVIs created for each VLAN  
- Have routing enabled (`ip routing`)  
- Have end devices pointing to the SVI as their default gateway  

---

# 🛑 9. Key Differences: Day 14 vs Day 15

| Feature | Day 14 – Router-on-a-Stick | Day 15 – L3 Switch Routing |
|---------|-----------------------------|-----------------------------|
| Device Used | Router | L3 Switch |
| Method | Sub-interfaces | SVIs |
| Speed | Slow (software) | Fast (hardware) |
| Bottleneck | Single trunk | None |
| Best For | Small networks | Medium/large networks |
| Real-world use | Rare today | Very common |

---

# 🔍 10. Example Network Design (Realistic)

BlueWave University upgrades its switches:

- Teachers → VLAN 10  
- Students → VLAN 20  
- Admin → VLAN 30  
- Cameras → VLAN 40  
- WiFi → VLAN 50  

Instead of connecting all VLANs to a single router,  
the L3 switch handles routing instantly inside the switch.

This is standard in:

- Universities  
- Corporate offices  
- Hospitals  
- Banks  
- Data centers  

---

# 📝 11. Reminder

This is the **README.md** file for Day 15.  
All commands and configurations are provided in:

➡ **Day 15 configuration.md**
