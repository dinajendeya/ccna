
# Day 17 – Spanning Tree Protocol (STP) Load Balancing  
*How to Use STP to Load‑Balance Traffic by Making Each Switch the Root for Different VLANs*  
> **Note:** All configuration commands are provided separately in  
> **Day 17 – STP Load Balancing – configuration.md**  
> This file focuses ONLY on the concepts.  
> Day 16 covered classic STP.  
> Day 18 will cover Rapid STP (RSTP) and fast convergence.

---

# 🧠 1. The Problem: STP Wastes Bandwidth

In **Day 16**, we saw that STP prevents loops by **blocking redundant links**.

But here’s the problem:

### ❌ STP blocks one link entirely  
If you have two uplinks between switches:

```

Switch A === Switch B

```

STP will:

- Forward on one link  
- Block the second link  

That second link does **nothing** unless the first link fails.

➡️ *50% of available bandwidth is wasted.*

In big campus networks, this is a huge disadvantage.

But there is a solution…

---

# 💡 2. The Solution: Per‑VLAN STP Load Balancing

You can configure **different Root Bridges for different VLANs**.

This makes devices in different VLANs use different links.

Example:

- VLAN 10 uses Link 1 (because Switch A is root)
- VLAN 20 uses Link 2 (because Switch B is root)

This lets STP **distribute** traffic across multiple redundant paths.

✔ No loops  
✔ No blocked backup-only links  
✔ Both links are actively forwarding  
✔ Full bandwidth utilization  

This technique is called:

### ⭐ Per‑VLAN STP Load Balancing  
(or simply STP Load Balancing)

---

# 🏫 3. High‑School Analogy

Imagine two hallways connecting two buildings.

Before STP Load Balancing:

- Only one hallway is open  
- The other is blocked  
- Students crowd into one path  
- The other hallway is wasted

With STP Load Balancing:

- Students from Floor 1 use Hallway A  
- Students from Floor 2 use Hallway B  
- Both hallways are used  
- No crowding  
- Backup options remain available  

This is the exact logic of STP per‑VLAN load balancing.

---

# 🧩 4. How STP Load Balancing Works (Concept)

### Step 1  
Your switches have at least two links between them → STP blocks one.

### Step 2  
You configure **different Root Bridges** per VLAN.

Example:

- Switch A → Root for VLAN 10  
- Switch B → Root for VLAN 20  

### Step 3  
STP recalculates for each VLAN separately:

- VLAN 10 chooses shortest path to Switch A  
- VLAN 20 chooses shortest path to Switch B  

### Step 4  
As a result:

- Link 1 forwards VLAN 10  
- Link 2 forwards VLAN 20  

Both links forward traffic → **Perfect load balancing**.

---

# 🏗 5. Typical Enterprise Design

Most companies use at least two distribution switches:

```

      +--------------+
      | Switch A     |  <-- Root for VLANs: 10, 30, 50
      +--------------+
          ||    ||
          ||    ||
          ||    ||
      +--------------+
      | Switch B     |  <-- Root for VLANs: 20, 40, 60
      +--------------+

```

Meaning:

- Half the VLANs prefer Switch A  
- Half the VLANs prefer Switch B  

This makes both uplinks **active and forwarding**.

---

# 🔥 6. Why Per‑VLAN Root Assignment Works

Because **STP is calculated per VLAN**.

So each VLAN becomes its own spanning tree.

Example:

- VLAN 10 → one spanning tree  
- VLAN 20 → another spanning tree  
- VLAN 30 → another, etc.

Each spanning tree can choose:

- A different root
- A different best path
- A different link to block or forward

This flexibility enables load balancing.

---

# 🔍 7. What Exactly Gets Balanced?

### ✔ Inter‑switch traffic  
VLANs flow via separate physical paths.

### ✔ Upstream routing  
Using different root switches avoids congestion.

### ✔ Downstream distribution  
Access switches choose different root ports for different VLANs.

### ✔ Broadcast & multicast  
These flows also get distributed.

---

# ⚠ 8. Important Rules of STP Load Balancing

### 1. Only Layer 2 links are used  
STP works below routing.

### 2. You MUST manually set priorities  
If you leave defaults, STP elects roots randomly.

### 3. At least two uplinks are needed  
Otherwise, no load balancing is possible.

### 4. Each VLAN requires its own Root Bridge  
If all VLANs use the same root, you gain nothing.

---

# 🧪 9. Example: How Traffic Flows with Load Balancing

Assume:

- **Switch A** is root for VLAN 10  
- **Switch B** is root for VLAN 20  

And two links:

- Link 1 → lower cost to Switch A  
- Link 2 → lower cost to Switch B  

Then:

- VLAN 10 forwards on Link 1  
- VLAN 20 forwards on Link 2  

Both links forward traffic → no blocking except for unused VLANs.

---

# 🔄 10. Relationship with Other Days

### Day 16  
Introduced STP basics:  
- Root Bridge  
- Port roles  
- Port states  
- BPDUs  

### **Day 17 (Today)**  
Using STP to:  
- Load balance VLANs  
- Distribute traffic  
- Make multiple links active  
- Increase performance & redundancy  

### Day 18  
Upgrading STP to **Rapid STP (RSTP)** for:  
- Faster convergence  
- Better port roles  
- Improved failure recovery  

---

# 📝 11. Reminder

This README contains **only explanations**.  
All required configurations — including:

- Setting root priorities per VLAN  
- Adjusting STP costs  
- Demonstrating load balancing results  
- Verification commands

…are provided in:

➡ **Day 17 configuration.md**