
# Day 32 – IPv6 Static Routing  
 
> **Note:**  
> This README.md explains the **theory only**.  
> All router command examples will be provided in:  
> **Day 32 – IPv6 Static Routing – Configuration.md**

---

# 🧠 1) The Story: Why Do We Need IPv6 Static Routing?

Think of IPv6 networks just like IPv4 networks — devices in different networks cannot talk to each other **unless routers know the path**.

Imagine three buildings at BlueWave University:

- Building A → IPv6 subnet `2001:DB8:10::/64`  
- Building B → IPv6 subnet `2001:DB8:20::/64`  
- Building C → IPv6 subnet `2001:DB8:30::/64`  

Each building has its own router.  
For them to communicate, **each router must know how to reach all other IPv6 subnets**.

There are two main ways to do this:

1. **Static Routing** → You manually define every path  
2. **Dynamic Routing** → Protocols like OSPFv3 handle everything automatically  

Day 32 is all about STATIC routing, the simplest foundational method.

---

# 🌍 2) What Is IPv6 Static Routing?

An IPv6 static route is a **manually configured** path telling the router:

> “To reach this IPv6 network, send traffic out this interface or to this next-hop IPv6 address.”

Unlike IPv4 static routes, IPv6 routes have slight differences:

- IPv6 often uses **link-local addresses** as next-hops  
- All IPv6 routers must have **link-local addresses**  
- Interface must be specified when using link-local as next-hop  
- No NAT involved (IPv6 doesn’t need NAT)

Example IPv6 route format:

```

ipv6 route <destination-prefix> <next-hop> \[interface]

```

---

# 🧩 3) Types of IPv6 Static Routes

### ✔ 1) Direct Route (Next-hop global IPv6)
Simple static route using the next router’s **global unicast** address.

Example:
```

2001:DB8:20::/64 via 2001:DB8:10::2

```

---

### ✔ 2) Link-Local Next-Hop Route  
Most common in labs and CCNA exams.  
Uses the next router’s **FE80::** address.

Because link-local addresses are only valid on a single link, you MUST specify the **exit interface**.

Example:
```

ipv6 route 2001:DB8:20::/64 FE80::2 gig0/0

```

---

### ✔ 3) Default Route (::/0)
Equivalent to IPv4’s `0.0.0.0/0`.

Used when the router should send unknown destinations toward the ISP/upstream router.

Example:
```

ipv6 route ::/0 FE80::1 gig0/2

```

This is used heavily on edge routers pointing to an internet gateway.

---

### ✔ 4) Floating Static Route
A backup IPv6 static route with **higher administrative distance**.

Used only if the primary route disappears.

Example:
```

ipv6 route 2001:DB8:30::/64 FE80::5 gig0/1 50

```

Default admin distance for IPv6 static routes = 1  
Floating routes must be **higher** than the primary route.

---

# 🔥 4) Why IPv6 Static Routing Is Different From IPv4

### IPv6 static routing has:
- **Mandatory link-local next-hop capability**
- **No NAT required**
- **LLA as default gateway for hosts**
- Multicast-based Neighbor Discovery  
- Preferred use of `/64` LAN prefixes  
- SLAAC affecting host addressing

IPv6 static routing is both simpler (no NAT) and more strict (interface + LLA).

---

# 🧠 5) How the Router Decides the Path (Static Route Logic)

When an IPv6 packet arrives:

1. Router checks exact prefix match  
2. If no exact match → looks for longest prefix match  
3. If none → checks default route (::/0)  
4. If still none → packet dropped

This is identical to IPv4 longest prefix match routing.

---

# 🏫 6) Real‑World Example (BlueWave University)

Three routers:

- **R1** (VLAN10 – `2001:DB8:10::/64`)  
- **R2** (VLAN20 – `2001:DB8:20::/64`)  
- **R3** (VLAN30 – `2001:DB8:30::/64`)  

Links:

- R1 ↔ R2  
- R2 ↔ R3  

Goal: All networks must communicate.

Solution:

- R1 adds routes to networks behind R2 and R3  
- R2 adds routes to networks behind R1 and R3  
- R3 adds routes to networks behind R1 and R2  

This is the foundation for IPv6 routing before learning OSPFv3 or EIGRP for IPv6.

---

# 🧪 7) Verifying IPv6 Static Routes

Important commands (explained only here — configs next file):

### ✔ Show IPv6 routing table
```

show ipv6 route

```

### ✔ Show neighbors (NDP)
```

show ipv6 neighbors

```

### ✔ Ping using IPv6
```

ping ipv6 2001:DB8:20::1

```

### ✔ Trace using IPv6
```

traceroute ipv6 2001:DB8:30::5

```

### ✔ Check interface IPv6 details
```

show ipv6 interface gig0/0

```

---

# ⚠ 8) Common IPv6 Static Routing Mistakes

| Problem | Cause | Fix |
|--------|--------|------|
| Route not working | Using link‑local without interface | Always include the exit interface |
| Hosts cannot reach outside | Missing ::/0 default route | Add IPv6 default route |
| Neighbor Discovery issues | Missing link-local or interface down | Verify FE80:: and interface status |
| Wrong prefix | IPv6 prefixes must be exact | Use correct /64 LAN or upstream prefix |

---

# 🔄 9) How Day 32 Relates to Other IPv6 Topics

| Day | Topic |
|-----|--------|
| 31 | IPv6 Basics (addressing, link-local, SLAAC) |
| **32** | **IPv6 Static Routing** |
| 33 | OSPFv3 |
| 34 | IPv6 EIGRP |
| 35 | DHCPv6 Deep Dive |

Static routing is the backbone you need before moving to dynamic routing.

---

# 📝 10) Reminder

This README contains **no command examples**.  
All commands (global, link-local, ::/0, floating, and verification) are in:

➡ **Day 32 – IPv6 Static Routing – Configuration.md**
