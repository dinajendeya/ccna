
# Day 30 – PAT (Port Address Translation)  
  
> **Note:**  
> This README.md contains **only conceptual explanation**.  
> All configuration commands will be provided in:  
> **Day 30 – PAT – Configuration.md**

---

# 🧠 1) The Story: Why PAT Was Invented

By now, you’ve seen:

- **Static NAT** → 1 private IP ↔ 1 public IP  
- **Dynamic NAT** → temporary 1 private IP ↔ 1 public IP from a pool  

But both have a major limitation:

### ❌ They require *multiple* public IPs.

What if:

- You have **500 users**  
- But your ISP gives you **only ONE public IP**?

This is the situation for most homes, schools, small offices, and even many enterprises.

➡️ The solution is **PAT (Port Address Translation)**.  
PAT allows **hundreds or thousands** of internal devices to share *one* public IP.

---

# 🌍 2) What Is PAT?

**PAT** = Port Address Translation  
Also known as:

- **NAT Overload**  
- **Many‑to‑One NAT**  
- **NAT with ports**

PAT maps many internal private IPs to a single public IP by using **different port numbers**.

Example:

```

192.168.1.10:10500 → 203.0.113.1:10500
192.168.1.11:10501 → 203.0.113.1:10501
192.168.1.12:10502 → 203.0.113.1:10502

```

All three users share **203.0.113.1**, but PAT differentiates them using **unique port numbers**.

---

# 🎯 3) Why PAT Is the Most Common NAT Type

### ✔ Requires only ONE public IP  
Even for hundreds or thousands of devices.

### ✔ Supports massive scale  
Thanks to 65,535 available TCP/UDP ports.

### ✔ Automatically tracks sessions  
Each flow gets a unique mapping.

### ✔ Perfect for outbound browsing  
Users can access the Internet without needing public IPs.

### ✔ Default NAT mode used in homes, offices, schools, and enterprises

PAT is the reason IPv4 has survived so long.

---

# 🧩 4) How PAT Works Internally (Step‑by‑Step)

Let’s say:

- Public IP = 203.0.113.1  
- Inside host = 192.168.50.20  

The host sends traffic:

1. 192.168.50.20 → 8.8.8.8  
2. Router receives the packet  
3. Router creates a PAT entry:  
```

192.168.50.20:40001 → 203.0.113.1:40001

```
4. Router sends packet to Internet using public IP  
5. Google responds to 203.0.113.1:40001  
6. Router translates it back to the internal host

Each internal device uses a **different port**, so the router keeps track of all flows.

---

# 🔢 5) PAT Mapping Table Example

Inside Device → PAT Entry:

| Inside | Port | Outside | Port |
|--------|-------|----------|-------|
| 192.168.50.10 | 44000 | 203.0.113.1 | 44000 |
| 192.168.50.11 | 44001 | 203.0.113.1 | 44001 |
| 192.168.50.30 | 44002 | 203.0.113.1 | 44002 |

Router keeps this mapping until sessions end.

---

# 📌 6) PAT Vocabulary

| Term | Meaning |
|------|----------|
| Inside local | private IP |
| Inside global | public IP used by PAT |
| Port overloading | using port numbers to track multiple translations |
| NAT table | dynamic mapping record maintained by router |

---

# 🧪 7) Real‑World Example (BlueWave University)

BlueWave has:

- 500 students  
- 1 public IP: `203.0.113.1`

If they used Static or Dynamic NAT:

- They’d need 500 public IPs  
- Impossible and expensive

With PAT:

- All 500 students can browse simultaneously  
- Router creates 500 different port mappings  
- Only one IP is used

PAT is the only practical solution in this scenario.

---

# 🔒 8) Security Perspective

PAT provides **basic masking** of internal IPs, but it is NOT a firewall.

- It hides private IPs  
- It prevents unsolicited inbound traffic  
- But it does NOT filter or detect malicious traffic  
- Combine with ACLs or firewalls for real security

---

# 🔄 9) How PAT Differs From Other NAT Types

| Feature | Static NAT | Dynamic NAT | PAT |
|--------|-------------|-------------|------|
| Mapping | 1:1 | Temporary 1:1 | Many:1 |
| Public IP usage | High | Medium | Very Low |
| Requires public pool | Yes | Yes | No |
| Supports 100+ users | No | No | ✔ Yes |
| Allows inbound | Yes | No | No (unless port-forwarding) |

PAT is the only scalable NAT method.

---

# 🎮 10) PAT and Port Forwarding

PAT blocks inbound connections by default.  
If you want external access to a device inside the network, you must configure **static NAT with a port**.

Example:

```

ip nat inside source static tcp 192.168.1.10 80 203.0.113.1 80

```

This is called **Port Forwarding**.

---

# ⚠ 11) Common PAT Problems

- Missing default route  
- Wrong inside/outside interfaces  
- ACL mismatches  
- NAT translation full (rare unless ports exhausted)  

---

# 🔄 12) Relationship With Surrounding Days

| Day | Topic |
|------|--------|
| 28 | Static NAT |
| 29 | Dynamic NAT |
| **30** | **PAT (most common NAT)** |
| 31 | IPv6 — NAT not required |

PAT completes the NAT series, showing the modern, efficient way IPv4 networks access the Internet.

---

# 📝 13) Reminder

This README includes **no configuration commands**.  
All commands will be included in:

➡ **Day 30 – PAT – Configuration.md**

