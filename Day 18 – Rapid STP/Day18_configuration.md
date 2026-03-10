# Day 18 – Rapid Spanning Tree Protocol (RSTP) – configuration.md
*Practical RSTP configuration for Cisco switches (802.1w), including explanations, real scenarios, expected outputs* 

---
# 📘 1. Scenario Overview
Company: BlueWave University
Devices: Cisco Catalyst Switches (supporting RSTP)

Goal:
- Enable Rapid Spanning Tree Protocol (RSTP)
- Convert legacy STP to fast 802.1w
- Ensure instant recovery (1–2 seconds) from link failures
- Protect access ports with Edge + BPDU Guard
- Verify fast convergence

Network Topology:
- SW1 ↔ SW2 ↔ SW3 (Distribution layer)
- SW3 connects to access layer devices
- Multiple uplinks exist → RSTP handles rapid failover

---
# 🛠 2. Enable RSTP (802.1w) Globally
RSTP replaces STP immediately.

```bash
configure terminal
spanning-tree mode rapid-pvst
end
```

Explanation:
- `rapid-pvst` = Cisco's per-VLAN implementation of RSTP
- Each VLAN gets its own fast spanning tree
- Provides best performance in enterprise/campus networks

---
# 🛠 3. Configure Edge Ports (Equivalent to PortFast)
RSTP Edge Ports transition instantly to forwarding state.

Apply on all access (PC/user) ports:

```bash
configure terminal
interface range fa0/1 - 24
 spanning-tree portfast
 spanning-tree bpduguard enable
end
```

Explanation:
- `portfast` = immediate forwarding for host ports
- `bpduguard enable` = security; shuts port if a BPDU is received

---
# 🛠 4. Configure Uplink Interfaces (Point-to-Point Links)
RSTP performs fastest convergence on **point-to-point full-duplex** links.

On uplinks between switches:

```bash
configure terminal
interface gig0/1
 duplex full
 spanning-tree link-type point-to-point

interface gig0/2
 duplex full
 spanning-tree link-type point-to-point
end
```

Explanation:
- RSTP detects point-to-point links for faster negotiation
- Ensures sub-second failover with proposal/agreement mechanism

---
# 🛠 5. Optional: Tuning Priority for Faster Root Elections
Make SW1 the Root Bridge for **all VLANs**:

```bash
configure terminal
spanning-tree vlan 1-4094 priority 4096
end
```

Explanation:
- RSTP still uses Bridge Priority to elect a Root Bridge
- Setting low priority forces deterministic root assignment

---
# 🧪 6. Verification Commands

### ✔ Check RSTP Status
```bash
show spanning-tree
```
Expected:
```
Spanning tree enabled protocol rstp
```

### ✔ Check per-VLAN RSTP operation
```bash
show spanning-tree vlan 10
```
Expected outputs include:
- Port roles (Root, Designated, Alternate)
- States (Discarding, Learning, Forwarding)
- Edge/Network status

### ✔ Confirm Edge Ports
```bash
show spanning-tree interface fa0/5 detail
```
Expected:
```
PortFast (Edge) enabled
BPDU Guard enabled
```

### ✔ Confirm Link Types
```bash
show spanning-tree interface gig0/1 detail
```
Expected:
```
Link type: Point-to-Point
```

---
# 🔍 7. Expected Behavior (In Real Network)
- Link failures recover in **1–2 seconds**
- No extended Listening/Learning delays
- Edge ports never cause topology recalculations
- Alternate ports activate instantly replacing root ports
- Rapid, stable convergence across all VLANs

---
# ⚠ Common RSTP Mistakes & Avoidance
| Mistake | Result | Prevention |
|---------|--------|------------|
| Mixing STP & RSTP switches | Causes fallback to slow STP | Ensure all switches run RSTP
| No BPDU Guard | Accidental loops from user ports | Enable BPDU Guard on all Edge ports
| Half-duplex uplinks | RSTP cannot classify link properly | Ensure full-duplex on trunk/uplink ports
| Forgetting `rapid-pvst` mode | Switch remains in slow STP | Explicit global configuration

---
# 🏁 8. Final Summary
This configuration file includes:
- Enabling RSTP (rapid-pvst)
- Edge + BPDU Guard configuration
- Point-to-point uplink tuning
- Priority tuning for deterministic root elections
- Complete verification commands
- Expected network behavior under RSTP

No overlap with:
- Day 16 (Classic STP)
- Day 17 (STP Load Balancing)

RSTP is now fully implemented with fast, modern convergence.
