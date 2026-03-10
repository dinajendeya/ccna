# Day 19 – Port Security – Configuration.md
*Complete configuration for Port Security with real scenarios, explanations, verification steps.*

---
# 📘 1. Scenario Overview
Company: BlueWave University
Device: Cisco Catalyst 2960/3560 (Layer 2/Layer 3 capability irrelevant for Port Security)

Goal:
- Secure access ports using Port Security
- Prevent unauthorized devices from connecting
- Use sticky MAC for easy management
- Configure violation actions
- Automatically shut down rogue devices

Scenario:
Each classroom port should:
- Allow only **1 MAC address**
- Learn MAC automatically (sticky)
- Shut down if an unknown device connects

---
# 🛠 2. Basic Port Security Configuration (Sticky + Shutdown)
Apply to a single port (example: Fa0/10):

```bash
configure terminal

interface fa0/10
 switchport mode access
 switchport access vlan 30

 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown

end
```

Explanation:
- Port can learn **one MAC address**
- Sticky MAC stores learned MAC in running‑config
- If another device appears → port shuts down

---
# 🛠 3. Apply Port Security to a Range of Ports
Example: All classroom ports Fa0/1–Fa0/20

```bash
configure terminal

interface range fa0/1 - 20
 switchport mode access
 switchport access vlan 30

 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown

end
```

---
# 🛠 4. Configure Static MAC Address (Optional)
Used when you want to **manually bind** a device to a port.

```bash
configure terminal
interface fa0/15
 switchport mode access
 switchport access vlan 30
 switchport port-security
 switchport port-security mac-address 00A1.B2C3.D4E5
 switchport port-security violation restrict
end
```

Explanation:
- Only the specified MAC address is allowed
- Violation mode = **restrict** → logs & counter updates, but port stays active

---
# 🛠 5. Violation Modes Summary
| Mode | Behavior |
|------|----------|
| **protect** | Drops unauthorized frames silently |
| **restrict** | Drops frames + logs + SNMP traps |
| **shutdown** | Port is disabled (error‑disabled) |

Shutdown mode is most secure and the default.

---
# 🛠 6. Recovering a Port from Violation Shutdown
When a port is placed in **err‑disabled** state, recover manually:

```bash
configure terminal
interface fa0/10
 shutdown
 no shutdown
end
```

Or enable **automatic recovery**:

```bash
configure terminal
errdisable recovery cause psecure-violation
errdisable recovery interval 30
end
```

---
# 🧪 7. Verification Commands
### ✔ Check Port Security on a Specific Interface
```bash
show port-security interface fa0/10
```
Expected:
- Port status: Secure‑up
- Maximum MACs: 1
- Sticky MAC learned

### ✔ Display All Secure MAC Addresses
```bash
show port-security address
```
Expected:
- List of learned/sticky/static MACs

### ✔ Check Violation Counters
```bash
show port-security
```
Useful to detect attacks or unauthorized devices.

---
# 🔍 8. Expected Real‑World Behavior
- If a student plugs a laptop → port shuts down instantly
- MAC address learned becomes sticky → saved in running config
- If saved to NVRAM, MAC persists after reboot
- Prevents rogue switches from expanding the LAN

---
# ⚠ Common Port Security Mistakes & Avoidance
| Mistake | Problem | Fix |
|---------|----------|------|
| Using port-security on trunk ports | Breaks network | Use only on access ports |
| Setting maximum > 1 | Allows multiple devices | Keep maximum = 1 |
| Forgetting sticky mode | MAC resets after reload | Enable sticky |
| No BPDU Guard | Rogue switches connect | Combine with BPDU Guard |

---
# 🏁 9. Final Summary
This configuration file includes:
- Sticky, static, and dynamic MAC configurations
- Violation modes
- Manual & automatic recovery
- Proper access port design
- Verification outputs
- Real‑world expected behavior
