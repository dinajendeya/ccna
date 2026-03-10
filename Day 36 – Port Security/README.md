
# Day 19 – Port Security  

> **Note:** All configuration commands will be provided separately in  
> **Day 19 – Port Security – configuration.md**  
> This README focuses ONLY on understanding the concept.

---

# 🧠 1. The Story: Why Port Security Was Invented

Imagine you’re in a school computer lab.  
Every computer has an Ethernet port connected to the school switch.

Now imagine:

- A student unplugs the cable from the school computer  
- And plugs it into **their own laptop**  
- Or a hacker connects a device outside the classroom  
- Or someone plugs in another switch with its own configuration and expands the network  

This creates huge risks:

### ❌ Unauthorized access  
Someone gains access to internal resources.

### ❌ MAC flooding attacks  
A hacker floods the switch with fake MAC addresses → the switch “fails open” → sends traffic to all ports → attacker can sniff everything.

### ❌ Instability  
Plugging in an unmanaged switch may create loops or broadcast storms.

Network admins needed a way to **lock** a switch port to the device that is *supposed* to be there.

➡️ That solution is **Port Security**.

---

# 🔐 2. What Is Port Security?

**Port Security is a Layer 2 security feature that allows you to control which devices are allowed to send traffic through a specific switch port.**

It works by limiting:

- **Which MAC addresses** can use the port  
- **How many MAC addresses** the port is allowed to learn  
- **What happens** if an unauthorized device appears  

Port Security = a “bouncer” at each port.

---

# 🏫 3. High‑School Analogy

Imagine each classroom has a door with:

- A list of allowed students  
- A maximum number of people allowed  
- A rule:  
  - If someone unknown enters → block the door  
  - Or warn the teacher  
  - Or remove the student  

That’s exactly how Port Security behaves.

---

# 🧩 4. Why Port Security Is Important

### ✔ Protects against unauthorized devices
Stops people from plugging in rogue laptops or rogue switches.

### ✔ Prevents MAC flooding attacks
Limits how many MACs a port can learn.

### ✔ Prevents accidental network loops
A student connecting a cheap switch won’t break the network.

### ✔ Adds a security layer inside your LAN
Even if someone physically accesses a port, they still can’t connect.

### ✔ Required for campus networks, banks, hospitals, and enterprise networks
Highly essential for Zero‑Trust security.

---

# 🔍 5. How Port Security Works

When activated, a port can:

### 1️⃣ Limit the number of learned MAC addresses  
Example:
- Allow only **1 device** per port  
- If a second device appears → violation occurs  

### 2️⃣ Bind a specific MAC address  
The switch can “remember” the MAC address that belongs to this port:

- Statically assigned  
- Dynamically learned  
- Sticky (learn and save automatically)

### 3️⃣ Take action when a violation happens  
Violation actions:

| Action | Behavior |
|--------|----------|
| **Protect** | Drops unauthorized frames silently |
| **Restrict** | Drops frames + sends logs/SNMP traps |
| **Shutdown** | Port goes error‑disabled (most secure) |

**Shutdown** is the default and most common.

---

# 🔧 6. Types of Allowed MAC Addresses

### ✔ Static MAC
Admin manually configures:
```

MAC A is allowed on port F0/1 only.

```

### ✔ Dynamic MAC
Switch learns MAC automatically but does not save it.

### ✔ Sticky MAC  
Switch learns the MAC and **writes it into the running configuration**.  
Survives reloads when saved.

Sticky is the most popular in real networks.

---

# 💥 7. What Causes a Violation?

A violation occurs if:

- A different MAC tries to use the port  
- More MAC addresses appear than the allowed limit  
- A device is replaced without updating the sticky MAC list  
- A rogue switch is connected  
- Someone tries MAC spoofing  

When a violation happens:

- Port blocks traffic  
- Or port shuts down (shutdown mode)  
- Or logs an event depending on mode  

---

# 📡 8. Real‑World Example (BlueWave University)

BlueWave University configures Port Security on all classroom ports:

- Each classroom PC uses VLAN 30  
- Only **1 MAC address** is allowed per port  
- Sticky MAC is enabled  
- Violation mode: shutdown  

If a student plugs in their own laptop:

- Switch detects a new MAC  
- Port shuts down  
- An alert is logged  
- Admin must re-enable the port after investigation  

This prevents unauthorized access across the entire campus.

---

# 🚨 9. Why Port Security Is Not Enough Alone

Port Security protects **physical access**, but must be combined with:

- DHCP Snooping  
- Dynamic ARP Inspection  
- IP Source Guard  
- 802.1X Authentication  

We will see many of these in later days.

---

# 🔄 10. Relationship With Other Days

### Day 36 (Today)  
Port Security (Layer 2 endpoint protection)

### Future Days:  
- **Day 38 – DHCP Snooping** (protects against rogue DHCP)  


# 📝 11. Reminder

This README contains explanations only.  
All related configuration commands—covering:

- Static MAC  
- Dynamic MAC  
- Sticky MAC  
- Violation modes  
- Recovery from shutdown  
- Real deployment scenarios  

…are provided separately in:

➡ **Day 36 Configuration.md**


