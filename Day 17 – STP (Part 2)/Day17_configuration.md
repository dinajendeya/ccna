# Day 17 – STP Load Balancing – configuration.md
*Configuration for Per‑VLAN STP Load Balancing using multiple uplinks.*

---
# 📘 1. Scenario Overview
Company: BlueWave University
Devices: Three Cisco Catalyst switches (SW1, SW2, SW3)

Goal:
- Use **two uplinks** between distribution switches.
- Avoid wasting bandwidth by blocking one link.
- Make each switch the **Root Bridge for different VLANs**.
- Achieve STP load balancing:
    - VLANs 10 & 30 prefer Switch A (Root A)
    - VLANs 20 & 40 prefer Switch B (Root B)

VLAN Layout:
- VLAN 10 – Teachers
- VLAN 20 – Students
- VLAN 30 – Admin
- VLAN 40 – Cameras

Links:
- SW1 <—> SW2 (Gi0/1)
- SW1 <—> SW3 (Gi0/2)
- SW2 <—> SW3 (Gi0/3)

Without STP Load Balancing → 1 link blocked.
With Load Balancing → Each VLAN uses a different path.

---
# 🛠 2. Make SW1 Root Bridge for VLANs 10 & 30
SW1 will handle routing and STP direction for those VLANs.

```bash
configure terminal

! Root for VLAN 10
tspanning-tree vlan 10 priority 4096

! Root for VLAN 30
spanning-tree vlan 30 priority 4096

end
```

Explanation:
- Default priority = 32768
- Lower = stronger
- 4096 ensures SW1 wins for VLANs 10 and 30

---
# 🛠 3. Make SW2 Root Bridge for VLANs 20 & 40
SW2 will forward traffic for these VLANs.

```bash
configure terminal

! Root for VLAN 20
spanning-tree vlan 20 priority 4096

! Root for VLAN 40
spanning-tree vlan 40 priority 4096

end
```

Now VLAN traffic splits:
- VLAN 10 & 30 follow SW1
- VLAN 20 & 40 follow SW2

---
# 🛠 4. Make SW3 a Backup Root for All VLANs
If SW1 or SW2 fails, SW3 takes over.

```bash
configure terminal
spanning-tree vlan 10,20,30,40 priority 8192
end
```

---
# 🛠 5. Adjust Port Costs to Control Path Selection
We fine‑tune traffic flow:
- Gi0/1 should prefer SW1
- Gi0/2 should prefer SW2

On SW3:
```bash
configure terminal

interface gig0/1
 spanning-tree vlan 10,30 cost 4

interface gig0/2
 spanning-tree vlan 20,40 cost 4

end
```

Effect:
- VLANs 10 & 30 → Link Gi0/1
- VLANs 20 & 40 → Link Gi0/2

Both uplinks become active.

---
# 🛠 6. PortFast + BPDU Guard on Access Ports
(Not used for load balancing but best practice.)

```bash
configure terminal
interface range fa0/1 - 24
 spanning-tree portfast
 spanning-tree bpduguard enable
end
```

---
# 🧪 7. Verification Commands

### Check Root Bridges
```bash
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
```
Expected:
- VLAN 10 & 30 Root → SW1
- VLAN 20 & 40 Root → SW2

### Check Which Links Are Forwarding or Blocking
```bash
show spanning-tree interface status
```
Expected:
- Gi0/1 forwarding for VLAN 10 & 30
- Gi0/2 forwarding for VLAN 20 & 40
- No link is 100% blocked for all VLANs

### Check BPDU Guard
```bash
show spanning-tree summary
```

---
# 🔍 8. Expected Behavior
- **No link is entirely wasted.**
- SW1 handles half the VLANs.
- SW2 handles the other half.
- SW3 uses:
  - Gi0/1 → VLAN 10 & 30
  - Gi0/2 → VLAN 20 & 40
- Broadcasts, unknown traffic, and multicast paths differ per VLAN.

---
# 🏁 9. Final Summary
This configuration file includes:
- Root Bridge for different VLANs
- Secondary Root for failover
- Load balancing via STP cost tuning
- Verification commands
- Expected results and behavior

No overlap with:
- Day 16 → Basic STP
- Day 18 → Rapid STP (RSTP)
