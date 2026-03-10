
# Day 33 – OSPFv3 (IPv6 OSPF)  

> **Note:** This README.md includes **concepts only**.  
> All configuration commands will be provided in:  
> **Day 33 – OSPFv3 – Configuration.md**

---

# 🧠 1) The Story: Why Do We Need OSPFv3?

In IPv4 networks, we used **OSPFv2** (Day 23 & 24) to dynamically learn routes.

But in IPv6 networks, routing behavior changes because:

- IPv6 uses **link‑local addresses**
- IPv6 has no broadcast
- IPv6 uses different neighbor discovery mechanisms
- IPv6 supports multiple addresses per interface by default  

So engineers created a new version of OSPF:

➡️ **OSPFv3** — designed specifically for IPv6.

It works similarly to OSPFv2, but modified for the IPv6 world.

OSPFv3 gives IPv6 networks:

- Fast convergence  
- Hierarchical design via areas  
- SPF (Dijkstra) algorithm  
- Scalability  
- Link-state database (LSDB) per area  

OSPFv3 is the **standard IPv6 dynamic routing protocol**.

---

# 🌍 2) What Is OSPFv3?

**OSPFv3 = OSPF for IPv6.**  
Defined in **RFC 5340**, OSPFv3 is the next generation of OSPF that supports IPv6.

Important changes from OSPFv2:

### ✔ Uses **IPv6 link‑local addresses** for neighbor adjacencies  
No more global unicast for next‑hop.

### ✔ Runs **on a per‑interface basis**, not via “network” statements  
Meaning: You enable OSPFv3 *directly on interfaces*.

### ✔ Supports multiple IPv6 prefixes on the same interface  
Each prefix can be advertised or filtered independently.

### ✔ Uses multicast instead of broadcast  
- FF02::5 → All OSPF routers  
- FF02::6 → All DR/BDR routers

### ✔ Updated LSAs  
LSA types changed to handle IPv6 topology.

---

# 🧩 3) Key OSPFv3 Concepts (Same as OSPFv2)

Even though OSPFv3 is redesigned, the **core OSPF logic remains the same**:

- SPF (Shortest Path First) algorithm  
- Areas (Backbone Area 0 + others)  
- DR/BDR elections on multi‑access networks  
- Hello packets  
- LSAs and LSDB  
- Cost metrics  

If you understand OSPFv2, OSPFv3 will feel familiar.

---

# 🧬 4) What’s New in OSPFv3 (Compared to OSPFv2)?

### 🔸 1) **Link-local next-hop addressing**  
Routers use **FE80::/10** to form adjacencies.  
This is mandatory since link‑local stays valid even if global prefixes change.

### 🔸 2) **Addressing and topology separated**  
OSPFv3 does *not* carry IP addresses inside LSAs anymore.  
This allows multiple IPv6 prefixes per interface without changing the topology LSAs.

### 🔸 3) **Authentication removed from OSPF — uses IPv6 AH/ESP instead**  
Security is handled by **IPsec** natively, not OSPF commands.

### 🔸 4) **Runs as its own process separate from OSPFv2**  
Example:  
- OSPFv2 → `router ospf 1`  
- OSPFv3 → `ipv6 router ospf 10`

You can run BOTH at the same time.

---

# 🔥 5) OSPFv3 Neighbor Formation Process

1. Routers send **Hello packets** to FF02::5  
2. Hello packets contain:  
   - Area ID  
   - Router ID (32-bit, same rules as IPv4 OSPF)  
   - Hello/dead timers  
   - Interface ID  
   - DR/BDR information  
3. If parameters match, adjacency forms  
4. DR/BDR elected (if multi‑access network)  
5. LSDB sync occurs  

Note: Even in IPv6, RID is still a **32-bit IPv4-looking number**, not IPv6.

---

# 🔍 6) OSPFv3 Router ID (RID)

Router ID is **not IPv6**, it still looks like IPv4:

```

1.1.1.1
2.2.2.2

```

OSPFv3 RID selection priority:

1. Manually configured RID  
2. Highest IPv4 address on active interface  
3. Highest IPv4 loopback  
4. If no IPv4 exists → OSPFv3 won’t start!

**Important:**  
Even pure IPv6 networks still require at least one IPv4 address *somewhere* to auto-generate a RID — or you must configure RID manually.

---

# 📡 7) OSPFv3 Network Types

Exactly the same as OSPFv2:

| Network Type | Behavior |
|--------------|----------|
| Point-to-Point | No DR/BDR |
| Broadcast Ethernet | DR/BDR election |
| Non-broadcast | Manual neighbors |
| Point-to-multipoint | No DR/BDR |

---

# 🧠 8) OSPFv3 Multicast Addresses

OSPFv3 uses IPv6 multicast exclusively:

| Purpose | IPv6 Multicast |
|----------|----------------|
| All OSPF routers | **FF02::5** |
| All DR/BDR | **FF02::6** |

There is **no broadcast** in IPv6.

---

# 🧪 9) Real‑World Example (BlueWave University)

BlueWave has three routers, each in IPv6 subnets:

- R1 → `2001:DB8:10::/64`  
- R2 → `2001:DB8:20::/64`  
- R3 → `2001:DB8:30::/64`  

Goal: Automatically exchange routes and calculate shortest paths.

OSPFv3 is enabled:

- On each interface  
- Using link‑local addresses  
- With Area 0 across all routers  

Routers form adjacencies, build an LSDB, and automatically route IPv6 packets between buildings.

---

# 🔄 10) Verifying OSPFv3

Important commands (concept only here):

```

show ipv6 ospf
show ipv6 ospf neighbor
show ipv6 ospf interface
show ipv6 route ospf
show ipv6 ospf database

```

You will use these heavily when troubleshooting.

---

# ⚠ Common OSPFv3 Issues

| Issue | Cause | Fix |
|--------|---------|------|
| No adjacency | area mismatch | match areas |
| No adjacency | link-local mismatch | verify LLAs |
| No OSPF routes | missing interface OSPF enable | enable OSPF per interface |
| RID is 0.0.0.0 | no IPv4 address | manually configure RID |
| DR/BDR unexpected | wrong interface priority | adjust OSPFv3 priority |

---

# 🔄 11) Relationship With Previous Days

| Day | Topic |
|-----|--------|
| 31 | IPv6 Basics |
| 32 | IPv6 Static Routing |
| **33** | **OSPFv3 (Dynamic Routing)** |
| 34 | IPv6 EIGRP |
| 35 | DHCPv6 Deep Dive |

OSPFv3 is the foundation for large IPv6 networks.

---

# 📝 12) Reminder

This README contains *no configuration commands*.  
All configs (OSPFv3 per-interface commands, RID setup, DR/BDR tuning, verification) will be provided in:

➡ **Day 33 – OSPFv3 – Configuration.md**
