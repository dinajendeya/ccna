# Day 43 – CDP & LLDP – configuration.md
*Practical configuration for neighbor discovery on Cisco IOS. Includes basic enable/disable, interface specifics, verification, and expected behavior. Aligned with CCNA scope.*

---
# 📘 1. Scenario Overview
Environment: **Cisco switches/routers** in a small campus.  
Goal: Make devices identify their neighbors using **CDP** (Cisco‑only) and **LLDP** (vendor‑neutral).  
Focus: Simple, safe settings you can use in labs and real networks.

> **Tip:** CDP is on by default on many Cisco devices; LLDP is usually off by default.

---
# 🛠 2. Global Controls
Enable/disable the protocols globally on a device.

## 2.1 CDP – Global
```bash
configure terminal
cdp run              ! enable CDP globally (default on many platforms)
! no cdp run         ! disable CDP globally
end
```

## 2.2 LLDP – Global
```bash
configure terminal
lldp run             ! enable LLDP globally (default is off on many platforms)
! no lldp run        ! disable LLDP globally
end
```

---
# 🛠 3. Interface‑Level Controls
Turn discovery on/off per interface; useful for security on external links.

## 3.1 CDP – Interface
```bash
configure terminal
interface GigabitEthernet1/0/1
 description User-Access-Port
 switchport mode access
 cdp enable          ! ensure CDP on this port
! no cdp enable      ! disable CDP on this port
end
```

## 3.2 LLDP – Interface
```bash
configure terminal
interface GigabitEthernet1/0/1
 description User-Access-Port
 lldp transmit       ! send LLDP advertisements
 lldp receive        ! accept/process LLDP from neighbor
! no lldp transmit
! no lldp receive
end
```

> **Best practice:** Disable CDP/LLDP on **untrusted/external** facing interfaces (e.g., internet edge) to avoid revealing device info.

---
# 🧪 4. Verification Commands

## 4.1 CDP – Verify & Inspect Neighbors
```bash
show cdp
show cdp neighbors
show cdp neighbors detail
show cdp interface
```
**Expected:** List of directly connected Cisco neighbors with **local/remote ports**, device IDs, capabilities, and (in *detail*) IP addresses and platform.

## 4.2 LLDP – Verify & Inspect Neighbors
```bash
show lldp
show lldp neighbors
show lldp neighbors detail
show lldp interface
```
**Expected:** Similar to CDP but **multi‑vendor**. Detail view shows system name, port ID, VLAN info, capabilities, and (if supported) **LLDP‑MED** TLVs for phones.

---
# 🔍 5. Expected Behavior in This Lab
- **CDP** discovers neighboring **Cisco** devices.
- **LLDP** discovers neighbors from **any vendor** supporting LLDP.
- Disabling CDP/LLDP on an interface stops advertisements and neighbor discovery on that link.

---
# ⚠ Common Misconfigurations & Fixes
| Issue | Symptom | Fix |
|------|---------|-----|
| LLDP not running | `show lldp neighbors` shows nothing | `lldp run` globally + enable on interfaces |
| CDP disabled on interface | Neighbor missing | `cdp enable` under the specific interface |
| Discovery left on external links | Information disclosure | `no cdp enable` / `no lldp transmit/receive` on internet‑facing ports |
| Phone not in voice VLAN | Poor call quality, wrong VLAN | Ensure `switchport voice vlan` + LLDP‑MED/CDP enabled |

---
# 🏁 Final Summary
This file provides:
- Global and interface controls for **CDP/LLDP**
- Optional timer tuning
- Verification commands and expected results
- Security‑minded guidance on where to enable/disable discovery

Use these snippets to quickly enable safe neighbor discovery and make troubleshooting much faster in your CCNA labs.
