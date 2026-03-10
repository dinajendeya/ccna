# Day 20 – EtherChannel – Configuration.md
*Complete EtherChannel configuration using LACP, PAgP, and Static mode with explanations, verification, and real-world scenario.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**
Devices: **Cisco Catalyst Switches** (2960/3560/3850)

Goal:
- Bundle multiple physical links into one logical Port-Channel
- Increase bandwidth and redundancy
- Prevent STP from blocking links
- Use LACP (recommended), PAgP (Cisco proprietary), and Static modes
- Ensure all interfaces match settings for a stable EtherChannel

Topology:
```
SW1 === SW2   (using 3 physical links)
```
Links used:
- Gi0/1
- Gi0/2
- Gi0/3

These will form **Port-Channel 1**.

---
# 🛠 2. EtherChannel Requirements (Important)
Before forming EtherChannel, **all interfaces must match**:
- Speed
- Duplex
- VLAN configuration
- Access or trunk mode
- Allowed VLANs (if trunk)
- Spanning-tree settings

Conflicts cause ports to be marked *suspended*.

---
# 🛠 3. Configure EtherChannel Using LACP (Recommended)
LACP Modes:
- **active** → actively negotiates
- **passive** → responds only

Use **active/active** for reliability.

### On SW1:
```bash
configure terminal

interface range gig0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 channel-group 1 mode active

end
```

### On SW2:
```bash
configure terminal

interface range gig0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 channel-group 1 mode active

end
```

Explanation:
- Ports join **Port-Channel 1** using LACP
- Trunk configuration is identical on both sides

---
# 🛠 4. Configure EtherChannel Using PAgP (Cisco Only)
PAgP Modes:
- **desirable** → actively negotiates
- **auto** → listens only

### SW1:
```bash
configure terminal
interface range gig0/1 - 3
 switchport mode trunk
 channel-group 1 mode desirable
end
```

### SW2:
```bash
configure terminal
interface range gig0/1 - 3
 switchport mode trunk
 channel-group 1 mode auto
end
```

---
# 🛠 5. Configure Static EtherChannel (No Protocol)
This works even if the other switch does not support LACP/PAgP.

### SW1 & SW2:
```bash
configure terminal
interface range gig0/1 - 3
 switchport mode trunk
 channel-group 1 mode on
end
```

⚠ Note: **Static mode does not detect mismatches** → be careful.

---
# 🛠 6. Configure Layer 3 EtherChannel (Optional)
Used between routers or Layer 3 switches.

### SW1:
```bash
configure terminal
interface range gig0/1 - 3
 no switchport
 channel-group 10 mode active

interface port-channel 10
 ip address 10.1.1.1 255.255.255.252
end
```

### SW2:
```bash
configure terminal
interface range gig0/1 - 3
 no switchport
 channel-group 10 mode active

interface port-channel 10
 ip address 10.1.1.2 255.255.255.252
end
```

---
# 🧪 7. Verification Commands
### Check EtherChannel Status
```bash
show etherchannel summary
```
Expected:
```
Group  Port-channel  Protocol
1      Po1(SU)       LACP
```
- **S** = Layer 2
- **U** = In use
- **P** = Bundled ports

### Check Member Interfaces
```bash
show etherchannel 1 port-channel
```

### Check Load Balancing
```bash
show etherchannel load-balance
```

### Check Trunk for Port-Channel
```bash
show interfaces trunk
```

---
# 🔍 8. Expected Real‑World Behavior
- If one physical link fails → Port-Channel stays up
- All VLANs continue to pass normally
- Traffic is distributed across available links
- STP sees one logical interface → no blocking of member links

---
# ⚠ Common EtherChannel Mistakes & Fixes
| Mistake | Problem | Solution |
|---------|----------|----------|
| Mismatched VLANs | Ports suspended | Ensure identical trunk settings |
| Mixing modes (LACP + PAgP) | EtherChannel fails | Use same protocol both sides |
| One port in access, one trunk | Bundle fails | Consistent mode on all ports |
| Different speed/duplex | Err-disabled | Match interface settings |

---
# 🏁 9. Final Summary
This configuration file includes:
- LACP, PAgP, Static EtherChannel
- Layer 2 & Layer 3 examples
- Verification commands
- Expected outputs & troubleshooting
- Real enterprise use case

No overlap with:
- STP topics (Days 16–18)
- Routing topics (starting Day 21)

EtherChannel is now fully implemented.
