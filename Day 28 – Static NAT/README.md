
# Day 28 – Static NAT (Network Address Translation)
 
> **Note:** This README.md focuses on concepts only.  
> All commands will be provided separately in:  
> **Day 28 – Static NAT – Configuration.md**

---

## 🧠 1) The Story: Why NAT Exists

In IPv4, public addresses are limited.  
Companies often use **private IP ranges** inside their networks (e.g., `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).  
These **private addresses are not routable on the Internet**.

**Problem:**  
How can a device on a private LAN communicate with servers on the Internet (which requires public IPs)?  
How can the outside world safely reach a server hosted inside a private LAN?

**Solution:**  
**NAT** (Network Address Translation) — the router changes IP addresses as packets pass through, allowing private hosts to talk to public networks.

---

## 🧩 2) What Is Static NAT?

**Static NAT** creates a **1‑to‑1 permanent mapping** between a private (inside local) IP and a public (inside global) IP.

Example mapping:

```

Inside server (private): 192.168.10.10  <—>  Public IP: 203.0.113.10

```

Any traffic from the Internet to **203.0.113.10** is translated to **192.168.10.10**, and vice versa.

**Key traits:**

- **Fixed**—mapping does not change.
- **Bidirectional**—supports both inbound and outbound connectivity.
- **Consumes a public IP per internal host** (not scalable for many clients).

---

## 🌍 3) NAT Vocabulary (Very Important for CCNA)

- **Inside local**: Private IP of the inside host (e.g., `192.168.10.10`).
- **Inside global**: Public IP that represents the inside host to the outside (e.g., `203.0.113.10`).
- **Outside global**: Public IP of the external host (e.g., `8.8.8.8`).
- **Inside interface**: Router interface facing the LAN (private side).
- **Outside interface**: Router interface facing the Internet (public side).

Remember: “inside”/“outside” describe **sides of the router**, and “local/global” describe **private vs public addressing**.

---

## 🎯 4) When to Use Static NAT

Use static NAT when you need **a consistent public address** for a specific internal host or service:

- Publicly accessible **web server** in your LAN
- **Mail server**, **VPN headend**, **VoIP gateway**
- Devices that must be **reachable from the Internet** at a fixed IP
- Special outbound cases where a host must always appear as the **same public IP**

If you only need many internal clients to browse the Internet, static NAT is **overkill**—use **PAT** (Day 30).

---

## 🔒 5) Security Perspective

NAT **is not a firewall**.  
Static NAT exposes a private host via a public IP; it **does not** automatically block ports.

For safe deployments, pair static NAT with:

- **ACLs** (Days 26–27) to allow/deny specific traffic
- **Zone‑Based Firewall** or an external firewall
- **Port‑forwarding (static NAT with ports)** to limit exposed services

---

## 🖧 6) How Static NAT Works (Step‑By‑Step)

**Scenario:** Internet client wants to reach your internal web server.

1. Internet client sends a packet to **203.0.113.10:80** (your static public IP).
2. NAT router receives it on the **outside** interface.
3. Router consults NAT table:  
   `203.0.113.10 ↔ 192.168.10.10`
4. Router **translates the destination** to **192.168.10.10:80** and forwards it to the LAN.
5. The server replies to the client; the router **translates the source** back to **203.0.113.10**.
6. Session flows normally in both directions.

Because the mapping is static, the router doesn’t need to “learn” or “allocate” anything dynamically.

---

## 🧪 7) Real‑World Example (BlueWave University)

- Internal web server: `192.168.10.10`
- ISP gave BlueWave: `203.0.113.10` (public)
- Goal: Make the site available on the Internet.

**Static NAT mapping:**  
`192.168.10.10 ↔ 203.0.113.10`

Add **ACLs** to allow only **TCP 80/443** from the Internet to the server.  
Users everywhere access **http(s)://203.0.113.10**, and NAT delivers the traffic to the internal server.

---

## 🧮 8) Static NAT vs Dynamic NAT vs PAT (Quick Compare)

| Feature | **Static NAT** | **Dynamic NAT** | **PAT (NAT Overload)** |
|---|---|---|---|
| Mapping | 1:1 fixed | 1:1 from a **pool** | Many:1 (ports) |
| Public IP usage | High | Medium | Low (best usage) |
| Inbound reachability | ✔ Excellent (fixed IP) | ✔ Possible (if reserved) | ❌ Not for inbound to specific host |
| Typical use | Public servers | Temporary 1:1 for subsets | Internet access for many clients |

Static NAT = best when **a host must be reachable** from outside on a fixed IP.  
PAT = best for **mass outbound Internet** access.  
Dynamic NAT = niche cases needing temporary 1:1 from a pool.

---

## 🧭 9) Common Pitfalls

- **Forgetting ACLs**: Static NAT alone doesn’t restrict traffic.
- **Wrong interface roles**: Marking inside/outside incorrectly breaks translation.
- **DNS mismatch**: Public DNS must point to the **public IP** (inside global), not the private one.
- **Overlapping subnets**: Private and public ranges must be correct and non-overlapping.
- **Return path**: Ensure routing back to the public IP reaches the NAT device.

---

## 🔄 10) Relationship With Neighboring Days

- **Day 26–27**: ACLs — often required to permit/limit inbound services with static NAT.  
- **Day 29**: **Dynamic NAT** — 1:1 mappings from a pool, allocated on demand.  
- **Day 30**: **PAT** — many internal clients share one public IP using ports.  
- **Day 37**: DHCP — often hands out the inside addresses that later get translated.

---

## 📝 11) Reminder

This README covers **concepts only**.  
For a ready‑to‑use configuration (interfaces, inside/outside roles, static mapping, and basic ACL protection), see:

➡ **Day 28 Configuration.md**
