# Day 08 – Switch Interfaces 

# 🔵 1. Cisco Switch Interface Basics
Switches operate at **Layer 2** of the OSI model. Their interfaces behave differently from routers.

### Key differences:
- **Switch interfaces are up/up by default** (unless shutdown).
- Routers start with interfaces **administratively down/down**.
- A switch port shows *down/down* only if **nothing is connected**.

---

# 🔵 2. Entering Privileged EXEC Mode
```
SW1> enable
```

---

# 🔵 3. Viewing Switch Interfaces
### Command:
```
SW1# show ip interface brief
```
This displays:
- Interface name
- IP (if SVI exists)
- Status (Layer 1)
- Protocol (Layer 2)

Switches normally show **up/up** for active ports.
Unconnected ports → **down/down**.

---

# 🔵 4. Checking Interface Status (Most Used Command)
```
SW1# show interfaces status
```
This shows:
- **Port** (Gi0/1, Fa0/12…)
- **Name** (description)
- **Status** (connected/notconnect)
- **VLAN** (default = VLAN 1)
- **Duplex** (auto/full)
- **Speed** (auto/100/1000)
- **Type** (copper/fiber)

Useful for quick diagnostics.

---

# 🔵 5. Interface Range Command
Used to configure **multiple ports at once**.

Example:
```
SW1(config)# interface range f0/5 - 12
SW1(config-if-range)# description ## not in use ##
SW1(config-if-range)# shutdown
```
This disables ports **0/5 to 0/12**.

### Why disable unused ports?
- Security (prevents unauthorized device connection)
- Reduces unnecessary broadcast traffic

To verify:
```
SW1# show interfaces status
```
Or in config mode:
```
SW1(config)# do show interfaces status
```

---

# 🔵 6. Full Duplex vs Half Duplex
### Half Duplex
- Cannot send and receive at the same time
- Requires **CSMA/CD** (collision detection)
- Historically used by **hubs**

### Full Duplex
- Can send and receive simultaneously
- No collisions
- Modern switches use **full duplex** by default

### Collision Domain
- All devices connected to a hub share a **collision domain** → collisions common
- Switches avoid collisions (each port is its own collision domain)

---

# 🔵 7. Speed / Duplex Auto-Negotiation
Switch ports usually default to:
```
speed auto
duplex auto
```
Devices negotiate the **fastest compatible settings**.

### If the OTHER device has auto-negotiation disabled:
- **Speed** → switch tries to match; if mismatch, may fall to slowest supported speed (10 Mbps)
- **Duplex** rules:
  - If running at **10 or 100 Mbps** → switch uses **half duplex**
  - If **1000 Mbps or more** → must use **full duplex**

This can cause:
- Collisions
- Late collisions
- CRC errors

---

# 🔵 8. Interface Error Counters
Show detailed counters:
```
SW1# show interfaces
```
Scroll to the bottom to see errors.

### Common error fields:
- **Runts**: too-small frames (< 64 bytes)
- **Giants**: oversized frames (> 1518 bytes)
- **CRC**: failed checksum (damaged frame)
- **Frame errors**: malformed frames
- **Input errors**: total of several error types
- **Output errors**: switch tried to send frame but failed

### Causes:
- Bad cables
- Duplex mismatch
- Collisions (old hubs)
- Electrical interference

---

# 🔵 9. Essential Day 9 Commands Summary
```
show ip interface brief
show interfaces status
show interfaces
interface range f0/x - y
shutdown
no shutdown
```

---
