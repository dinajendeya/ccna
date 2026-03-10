
# Day 29 – Dynamic NAT  
 
> **Note:**  
> This README.md explains the *theory only*.  
> All configuration commands are provided separately in:  
> **Day 29 – Dynamic NAT – Configuration.md**

---

# 🧠 1) The Story: Why Dynamic NAT Exists

In Day 28, we learned **Static NAT**, which creates a permanent 1‑to‑1 mapping:

```

192.168.10.10  ↔  203.0.113.10

```

But what if:

- You have **50 internal users**
- You only have **5 public IPs** from the ISP
- You don’t want a permanent mapping for each user
- You want each user to temporarily use one of the available public IPs

Static NAT cannot do this — it wastes public IPs.

➡️ **Dynamic NAT** solves the problem by creating temporary 1‑to‑1 mappings *only when needed*.

It’s like a small parking lot:

- You have 5 parking spots (public IPs)
- You have 50 employees (private hosts)
- Employees take a spot when they arrive
- When they leave, the spot becomes free for someone else

Dynamic NAT is efficient but limited by the size of the pool.

---

# 🧩 2) What Is Dynamic NAT?

**Dynamic NAT maps private IPs to a pool of public IPs on demand.**

- Mapping is **temporary**
- Mapping is created **only when traffic flows**
- Mapping is removed when idle
- If the pool is full → new users must wait

Example mapping:

```

LAN host: 192.168.20.10 → 203.0.113.2
LAN host: 192.168.20.15 → 203.0.113.3

```

But if all public IPs in the pool are used → no more translations until one frees up.

---

# 🌍 3) NAT Vocabulary Refresher

Dynamic NAT still uses the same terms:

- **Inside local** – private IP of the inside host  
- **Inside global** – public IP assigned from the NAT pool  
- **Outside global** – public IP of external host  
- **Inside interface** – LAN side  
- **Outside interface** – Internet side  

Understanding “inside local” ↔ “inside global” is the key to NAT.

---

# 🎯 4) When to Use Dynamic NAT

Dynamic NAT is appropriate when:

- You have **several public IPs** from ISP  
- You want **temporary** 1:1 translation  
- You do not need inbound connections  
- You want to conserve public IPs but avoid PAT  
- You want predictable outbound traffic (each connection uses a full public IP)

### Typical use cases:

- Schools/universities with small pools of public IPs
- Companies with a limited /29 or /28 public block
- When certain applications require non‑PAT translation
- When outbound anonymity is needed (each user gets a different IP)

---

# 🧠 5) Why Not Always Use Dynamic NAT?

Because Dynamic NAT still provides **only 1:1** mapping.

If you have:

- 200 users  
- 5 public IPs  

Then **only 5 users can access the Internet at the same time**.

That’s why most networks today prefer:

- **PAT (Day 30)** — supports hundreds/thousands of users with just **one** public IP.

Dynamic NAT is more restrictive than PAT, but more flexible than Static NAT.

---

# 📡 6) How Dynamic NAT Works (Step-by-Step)

1. A host from inside network sends traffic to the Internet  
   e.g., `192.168.20.10 → 8.8.8.8`

2. The router checks the Dynamic NAT rule:  
   “Is this traffic allowed to be translated?”

3. The router picks the **first available public IP** from the NAT pool  
   Example: `203.0.113.2`

4. The router creates a temporary mapping:  
   `192.168.20.10 ↔ 203.0.113.2`

5. Router forwards the packet to the Internet

6. Replies come back to the public IP, router then reverses translation

7. When idle, the mapping clears and becomes available for someone else

---

# 🧪 7) Real‑World Example (BlueWave University)

BlueWave has:

- 192.168.20.0/24 for student PCs  
- 203.0.113.2–203.0.113.6 (5 public IPs from ISP)

BlueWave wants:

- Students to access Internet using any of the 5 public IPs  
- No fixed mapping  
- No inbound connections (only outbound)

Dynamic NAT is perfect here.

Mapping examples during busy hours:

```

192.168.20.10 → 203.0.113.2
192.168.20.11 → 203.0.113.3
192.168.20.12 → 203.0.113.4
192.168.20.15 → 203.0.113.5
192.168.20.50 → 203.0.113.6

```

If a 6th student tries to browse the internet → translation fails until one mapping frees up.

---

# 🧮 8) Dynamic NAT vs Static NAT vs PAT

| Feature | Static NAT | Dynamic NAT | PAT (Overload) |
|--------|------------|--------------|----------------|
| Public IP usage | High | Medium | Very Low |
| Mapping | Fixed 1:1 | Temporary 1:1 | Many:1 using ports |
| Inbound access | Yes | No | No |
| Scalability | Low | Medium | Very High |
| Best for | Servers | Limited public pools | Internet browsing for many |

Dynamic NAT is the “middle ground” between Static NAT and PAT.

---

# ⚠ 9) Common Dynamic NAT Pitfalls

- Pool too small → users cannot browse  
- Wrong ACL → no traffic gets translated  
- Wrong pool subnet mask → translation errors  
- Missing default route → return traffic fails  
- Using Dynamic NAT expecting inbound connections (not supported)

Dynamic NAT is **outbound only** unless combined with static NAT rules.

---

# 🔄 10) Relationship With Next Days

- **Day 28:** Static NAT — permanent 1:1 mapping  
- **Day 29:** Dynamic NAT — temporary 1:1 mapping  
- **Day 30:** PAT — many‑to‑one, best scalability

NAT topics build on each other, preparing for IPv6 translation concepts later.

---

# 📝 11) Reminder

This README does **not** include configuration commands.  
All required configs (interfaces, ACLs, NAT pools, bindings, verification) are located in:

➡ **Day 29 – Dynamic NAT – Configuration.md**
