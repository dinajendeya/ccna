# Day 16 – Spanning Tree Protocol (STP) – configuration.md
*Configuration for Classic STP (802.1D) with explanations, real scenarios, expected outputs.*

---
# 📘 1. Scenario Overview
Company: BlueWave University
Devices: Cisco Catalyst Switches (Layer 2)

Goal:
- Ensure loop-free redundancy between switches
- Configure STP priority to control the Root Bridge
- Verify STP operation and port roles
- Demonstrate STP blocking and forwarding behavior

Topology (simplified):
Switch1 <—> Switch2 <—> Switch3
And an additional redundant link:
Switch1 <—> Switch3

Without STP, this creates a **Layer 2 loop**.
With STP, the network becomes stable.

---
# 🛠 2. Set the Intended Root Bridge (Switch1)
We manually set Switch1 as the root so STP does not choose a random switch.

```bash
configure terminal
spanning-tree vlan 1 priority 4096
end
```

Explanation:
- Default priority = 32768
- Lower = better
- 4096 almost guarantees Switch1 becomes Root Bridge

---
# 🛠 3. Set Secondary Root Bridge (Switch2)
If Switch1 fails, Switch2 should take over.

```bash
configure terminal
spanning-tree vlan 1 priority 8192
end
```

---
# 🛠 4. Configure Port Costs for Better Path Selection
In this scenario, Switch1 → Switch2 should be preferred over Switch1 → Switch3.

On Switch1:
```bash
configure terminal
interface gig0/1
 spanning-tree cost 4

interface gig0/2
 spanning-tree cost 19
end
```

Explanation:
- **Lower cost = preferred path**
- STP will use Gi0/1 as the designated path

---
# 🛠 5. Enable STP Portfast on Access Ports (NOT on trunk ports)
Portfast prevents PCs from waiting 30–50 seconds when booting.

```bash
configure terminal
interface range fa0/1 - 24
 spanning-tree portfast
 spanning-tree bpduguard enable
end
```

Explanation:
- `portfast` bypasses Listening/Learning
- `bpduguard` protects against accidental loops

---
# 🧪 6. Verification Commands

### ✔ Check STP Status
```bash
show spanning-tree
```
Expected output:
- Switch1 shows **This bridge is the root**
- Ports show their STP roles:
  - Root Port
  - Designated Port
  - Blocking Port (non-designated)

### ✔ Check Port Roles Only
```bash
show spanning-tree interface status
```

### ✔ Check BPDU Guard
```bash
show spanning-tree summary
```

Expected:
```
PortFast BPDU Guard is enabled
```

---
# 🔍 7. Expected STP Behavior in This Scenario
- Switch1 becomes **Root Bridge**
- Switch2 becomes **Secondary Root**
- Switch3 blocks its redundant link toward Switch1 or Switch2

Result:
✔ No loops
✔ Backup link available
✔ Traffic flows efficiently

---
# ⚠ Common STP Misconfigurations
| Issue | Why It Fails | How We Avoid It |
|-------|--------------|-----------------|
| Incorrect root selection | Random switch becomes root | Manually set priority |
| Portfast on trunk ports | Causes loops | Applied only to access ports |
| No BPDU Guard | Edge ports can form loops | BPDU Guard enabled |

---
# 🏁 Final Summary
This configuration file includes:
- Root Bridge and Secondary Root configuration
- STP cost tuning
- Portfast + BPDU Guard
- Verification and expected results
- No overlap with Day 17 or Day 18 (which cover STP load balance and Rapid STP)

