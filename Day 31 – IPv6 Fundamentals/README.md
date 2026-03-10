
# Day 31 – IPv6 Fundamentals  

> **Note:**  
> This README.md contains **concepts only**.  
> All IPv6 commands (addressing, routing, SLAAC, DHCPv6, link‑local, etc.) will be provided in:  
> **Day 31 Configuration.md**

---

# 🧠 1) The Story: Why IPv6 Was Created

By the early 2000s, the Internet had a huge problem:

### **IPv4 was running out of addresses.**

IPv4 has only **4.3 billion addresses**, and with:

- smartphones  
- laptops  
- smart TVs  
- IoT devices  
- cloud servers  
- millions of new users daily  

…it became clear that IPv4 could not scale forever.

Engineers needed:

- A MUCH bigger address space  
- Better auto‑configuration  
- Better routing  
- Built‑in security (IPsec mandatory in IPv6)  
- No more NAT headaches  
- Simpler packet processing  

The solution → **IPv6**

---

# 🌐 2) What Is IPv6?

IPv6 is the **next generation of IP**, designed to replace IPv4.

### Key characteristics:
- **128‑bit IP addresses**  
- A total of **340 undecillion addresses**  
  (that’s 340 trillion × trillion × trillion)
- No need for NAT  
- Hierarchical addressing  
- Built‑in multicast  
- Built‑in link‑local addressing  
- Automatic configuration (SLAAC)

IPv6 is the foundation of the Internet’s future.

---

# 🔢 3) IPv6 Address Format

IPv6 uses **hexadecimal** and **8 groups** of 16 bits:

Example:

```

2001:0DB8:0000:0000:0A10:BCFF:FE21:66A1

```

Each group is separated by colons.

---

# ✂️ 4) Shortening IPv6 Addresses (VERY Important)

### **Rule 1 – Remove leading zeros**
```

0DB8 → DB8
000A → A

```

### **Rule 2 – Replace one sequence of consecutive all‑zero groups with ::**
Example:

```

2001:0DB8:0000:0000:0A10:0000:0000:0099

```

Shortened:

```

2001:DB8::A10:0:0:99

```

Or even shorter:

```

2001:DB8::A10:0:0:99

```

**But remember:**  
➡️ `::` can only appear **once** in an IPv6 address.

---

# 🧩 5) Types of IPv6 Addresses

### ✔ **Global Unicast**  
Public IPv6 addresses (like public IPv4).  
Start with: `2000::/3`

Example:
```

2001:4860:4860::8888

```

### ✔ **Link‑Local (LLA)**  
Automatic address used inside a LAN.  
Always starts with: `FE80:: /10`

Example:
```

FE80::1A2B:3CFF:FE4D:5E6F

```

**Every IPv6 interface MUST have a link‑local address.**

### ✔ **Unique Local (ULA)** (Private IPv6)  
Similar to IPv4 private addresses.  
Start with: `FD00::/8`

Example:
```

FD00:1234::1

```

### ✔ **Multicast**  
Start with: `FF00::/8`  
Replaces IPv4 broadcast.

---

# 💡 6) No More Broadcasts!

IPv6 **does not use broadcast**.

Instead, it uses:

- **Multicast** (FF00::/8)  
- **Anycast**  
- **Link‑local scope**  

IPv6 is more efficient and reduces unnecessary traffic.

---

# 🔧 7) How Devices Get IPv6 Addresses

IPv6 supports three assignment methods:

---

## 1️⃣ SLAAC – Stateless Address Auto‑Configuration  
*The router tells the host how to configure itself.*

- Uses Router Advertisements (RA)  
- Automatically generates IPv6 address  
- No DHCP server required

Host uses:
- Prefix from the router  
- Its own interface identifier (based on MAC or random)

---

## 2️⃣ DHCPv6 – Stateful  
Like IPv4 DHCP:

- DHCPv6 server hands out full addresses  
- More control, logs, and management

---

## 3️⃣ Static Manual Assignment  
You manually assign IPv6 addresses.  
Used for servers, routers, and management.

---

# 🔍 8) IPv6 Address Structure (Important for CCNA)

**IPv6 = Prefix + Interface ID**

Example:

```

2001:DB8:ACAD:1::10
Prefix length: /64

```

- `/64` = network prefix  
- Remaining 64 bits = interface ID

**Most IPv6 LANs use /64 prefixes.**

---

# 📡 9) Link‑Local Addresses (FE80)

Every IPv6 interface **must** have an LLA.

Used for:

- Neighbor Discovery  
- OSPFv3 adjacency  
- EIGRP for IPv6  
- Router Advertisements  
- Routing updates

LLAs never leave the local network.

---

# 🔄 10) Neighbor Discovery Protocol (NDP)

IPv6 replaces ARP with **NDP**, which handles:

- Address resolution  
- Router discovery  
- Prefix discovery  
- Duplicate address detection (DAD)

NDP uses:

- ICMPv6  
- Multicast  
- Link‑local addresses

---

# 🛡 11) IPv6 and Security

IPv6 was designed with **IPsec support built‑in**, although real deployments may still require manual config.

Since IPv6 has no NAT by default, security relies heavily on:

- ACLs  
- Firewalls  
- RA Guard  
- DHCPv6 Guard  

---

# 🏫 12) Real‑World Example (BlueWave University)

BlueWave migrates from IPv4 to IPv6:

- Routers get global unicast IPv6 /64 prefixes  
- Each VLAN gets IPv6 subnet  
- Students’ devices autoconfigure using SLAAC  
- Servers use static IPv6  
- Network uses OSPFv3 for routing  

IPv6 provides enough addresses for thousands of devices without NAT.

---

# 🔄 13) What Comes Next?

Today (Day 31) → IPv6 Basics  
Next Days:

- **Day 32 – IPv6 Static Routing**  
- **Day 33 – IPv6 OSPFv3**  
- DHCPv6  
- NDP  
- Dual‑stack deployment  

IPv6 is huge — this day lays the foundation.

---

# 📝 14) Reminder

This README contains **NO configuration commands**.  
All IPv6 commands will be provided in:

➡ **Day 31 Configuration.md**

