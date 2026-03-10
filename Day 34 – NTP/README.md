
# Day 34 – EIGRP for IPv6 (EIGRPv6)  

> **Note:**  
> This README.md focuses entirely on **EIGRP for IPv6** concepts.  
> All configuration commands will be provided in:  
> **Day 34 – EIGRP for IPv6 – Configuration.md**

---

# 🧠 1) The Story: Why EIGRP Needed an IPv6 Version

BlueWave University successfully deployed IPv6 and used:

- **Static Routing** (Day 32) — great for small networks  
- **OSPFv3** (Day 33) — scalable but slightly complex  

However, their Cisco-based network previously relied heavily on **EIGRP** for IPv4 because it is:

- Fast  
- CPU-efficient  
- Very stable  
- Easy to configure  
- Highly flexible  

They wanted **the same benefits in IPv6**, so Cisco created:

➡️ **EIGRP for IPv6 (EIGRPv6)**

It retains the *exact same logic* as IPv4 EIGRP but adapts to IPv6 architecture.

---

# 🌍 2) What Is EIGRP for IPv6?

**EIGRPv6 = EIGRP redesigned to support IPv6 natively.**

It is still:

- A Cisco proprietary protocol  
- A hybrid (advanced distance vector)  
- Based on the **DUAL algorithm**  
- Metric = Bandwidth + Delay (default)  
- Fast converging  
- Loop-free  

But adds IPv6-specific improvements.

---

# 🔍 3) Key Differences: EIGRP for IPv4 vs EIGRP for IPv6

| Feature | EIGRP for IPv4 | EIGRP for IPv6 |
|--------|------------------|----------------|
| Enabled under router mode? | Yes | **No (must be enabled per-interface)** |
| Use of network statements? | Yes | **No** |
| Address type | IPv4 | IPv6 |
| Router ID required | Yes | **Yes (still IPv4-looking)** |
| Multicast address | 224.0.0.10 | **FF02::A** |
| Default route format | 0.0.0.0/0 | **::/0** |
| Protocol number | 88 | 88 |
| Supports manual link-local next-hops | No | **Yes** |

The biggest change:

### 🚨 EIGRP for IPv6 is enabled directly ON THE INTERFACE, not through network statements!

---

# 🧩 4) Requirements Before EIGRPv6 Works

1. **IPv6 must be enabled globally**  
```

ipv6 unicast-routing

```

2. **Router-ID must be set**  
EIGRPv6 (like OSPFv3) requires a 32-bit RID.

3. **EIGRPv6 must be enabled globally**  
```

ipv6 router eigrp <AS>

```

4. **EIGRPv6 must be enabled per-interface**  
```

ipv6 eigrp <AS>

```

5. **Interfaces must have IPv6 addresses**  
(link-local & global)

---

# 🌐 5) EIGRPv6 Neighbors

EIGRPv6 routers form neighbor adjacencies using:

- Multicast address: **FF02::A**
- Link-local addresses (**FE80::/10**)
- Hello packets every 5 seconds (default)

Neighbor requirements:

- Same autonomous system (AS)  
- Same K-values  
- Same hello/hold timers  
- Link-local reachability  
- Interfaces must be in UP state  

Once neighbors form, DUAL calculates successor and feasible successor routes.

---

# 🧬 6) EIGRPv6 Metrics (Same as IPv4)

EIGRPv6 metric formula uses:

- **Bandwidth**  
- **Delay**  
- **Reliability** (optional)  
- **Load** (optional)  
- **MTU** (informational)  

Default K-values:  
```

K1 = 1  (bandwidth)
K2 = 0
K3 = 1  (delay)
K4 = 0
K5 = 0

```

This means:
> EIGRPv6 metric = bandwidth + delay (like IPv4 EIGRP)

---

# 🔥 7) Route Types in EIGRPv6

- **C** → Connected  
- **L** → Local  
- **D** → EIGRP learned  
- **EX** → External EIGRP (redistributed)  

Example:

```

D   2001:DB8:20::/64  \[90/2172416] via FE80::2, Gig0/1

```

---

# 🛰 8) Real‑World Example (BlueWave University)

Three routers:

- R1 → IPv6 LAN 2001:DB8:10::/64  
- R2 → IPv6 LAN 2001:DB8:20::/64  
- R3 → IPv6 LAN 2001:DB8:30::/64  

Each connected by IPv6 point-to-point links.

EIGRPv6 is activated per-interface:

- R1 enables EIGRPv6 on its LAN and uplink  
- R2 does the same on all three interfaces  
- R3 on its LAN and uplink  

Routes automatically exchange using DUAL, giving:

- Fast convergence  
- Loop-free routes  
- Simple config  
- Efficiency similar to IPv4 EIGRP

---

# 🧪 9) Important Verification Commands

(Concept only here — exact outputs in config file)

```

show ipv6 eigrp neighbors
show ipv6 eigrp topology
show ipv6 route eigrp
show ipv6 protocols
show ipv6 eigrp interfaces

```

---

# ⚠ 10) Common EIGRPv6 Issues

| Issue | Cause | Fix |
|--------|----------|--------|
| No neighbors | EIGRP not enabled on interface | Add `ipv6 eigrp <AS>` |
| Link-local mismatch | Wrong interface or LLAs not configured | Check `show ipv6 interface` |
| No routes learned | Missing Router-ID | Configure RID |
| Stuck in EXSTART | K-values mismatch | Use default K-values |
| Interface down | Admin shut or physical issue | Re-enable |

---

# 🔄 11) Relationship With Other Days

| Day | Topic |
|-----|--------|
| 33 | OSPFv3 |
| **34** | **EIGRPv6** |
| 35 | DHCPv6 |
| 36 | IPv6 Security & RA Guard |
| 37 | IPv6 Tunneling (GRE, IPsec) |

EIGRPv6 is the *Cisco-friendly* dynamic IPv6 routing protocol — very common in CCNA labs.

---

# 📝 12) Reminder

This README contains **zero configuration commands**.  
All commands will be included in:

➡ **Day 34 – EIGRP for IPv6 – Configuration.md**

