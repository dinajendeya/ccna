# Day 42 – SNMP & Syslog – configuration.md
*Simple, CCNA‑level configuration for SNMP and Syslog. Includes only the basics and quick verification.*

> ⚠ **Packet Tracer Limitation:** Packet Tracer does **not** provide real SNMP managers or external Syslog servers. You can enter commands on devices, but full end‑to‑end SNMP/Syslog testing requires GNS3/EVE‑NG/CML or real gear.

---
# 📘 1) Scenario Overview
- Devices: Cisco routers/switches
- Goal: Enable **basic monitoring** so a monitoring server can poll SNMP and collect Syslog
- Manager/Syslog server IP (example): **10.10.100.50**

We’ll show **either** SNMPv2c (simple) **or** SNMPv3 (secure), plus minimal Syslog.

---
# 🛠 2) SNMP – Quick Options
## A) SNMPv2c (simple, not encrypted)
```bash
configure terminal
! Identify the device (helps in NMS views)
hostname SW-IT-BLDG1
ip domain-name bluewave.local
!
! Community string (read-only). Choose a non-obvious string in real networks
snmp-server community BLUEWAVE-READ ro
!
! (Optional) Limit SNMP to the manager IP only
ip access-list standard SNMP-MANAGER
 permit 10.10.100.50
 deny any
snmp-server community BLUEWAVE-READ ro SNMP-MANAGER
!
end
```

## B) SNMPv3 (recommended, secure)
```bash
configure terminal
! Define an SNMPv3 group with auth+priv (encryption)
snmp-server group NMS-GRP v3 priv
!
! Create a local user bound to that group
! Replace <StrongPass> and <PrivKey> with strong secrets
snmp-server user nmsuser NMS-GRP v3 auth sha <StrongPass> priv aes 128 <PrivKey>
!
! (Optional) Restrict to manager IP
ip access-list standard SNMP-MANAGER
 permit 10.10.100.50
 deny any
snmp-server view SYSVIEW iso included
snmp-server group NMS-GRP v3 priv read SYSVIEW access SNMP-MANAGER
!
end
```

**Note:** Your SNMP manager must be configured to poll with **v2c + community** or **v3 + auth/priv** to match the option you choose above.

---
# 🛠 3) Syslog – Minimal Setup
Send logs to the central server and set a reasonable severity.

```bash
configure terminal
! Define the external Syslog server
logging host 10.10.100.50
!
! Send messages of level Informational (6) and higher severity (0–6)
logging trap informational
!
! (Recommended) Add timestamps and time sync
service timestamps log datetime msec
service timestamps debug datetime msec
!
! (Optional) Identify the device in messages
logging origin-id hostname
end
```

> Severity quick map: **0** Emergency ← **3** Error ← **4** Warning ← **5** Notice ← **6** Informational ← **7** Debug (noisy)

---
# 🔒 4) Access Control & Hardening (Quick Tips)
- Prefer **SNMPv3** for encryption
- If you must use v2c, use a **strong community** and ACL to **restrict by source IP**
- Limit Syslog acceptance on the server side with its own firewall rules
- Keep device clocks correct (NTP) so log timestamps make sense

---
# 🧪 5) Verification Cheatsheet
**SNMP**
```bash
show snmp
show snmp user
show snmp group
show access-lists SNMP-MANAGER
```

**Syslog**
```bash
show logging
show clock
```

**Generate a test message** (for local buffer; remote delivery depends on your lab):
```bash
configure terminal
logging userinfo
end
```
Then check `show logging` for a new entry.

---
# 🏁 6) Expected Results
- SNMP manager (10.10.100.50) can poll device status and interfaces
- Syslog server receives device events at level **informational** and higher
- In Packet Tracer you can enter commands, but external polling/collection won’t function

---
# 📌 Copy/Paste Blocks
## Use SNMPv2c + Syslog (simple)
```bash
configure terminal
hostname SW-IT-BLDG1
ip domain-name bluewave.local
snmp-server community BLUEWAVE-READ ro
ip access-list standard SNMP-MANAGER
 permit 10.10.100.50
 deny any
snmp-server location Building-1 MDF Rack-A
logging host 10.10.100.50
logging trap informational
service timestamps log datetime msec
service timestamps debug datetime msec
logging buffered 16384 informational
logging origin-id hostname
end
```

## Use SNMPv3 + Syslog (secure)
```bash
configure terminal
hostname SW-IT-BLDG1
ip domain-name bluewave.local
snmp-server view SYSVIEW iso included
snmp-server group NMS-GRP v3 priv read SYSVIEW
snmp-server user nmsuser NMS-GRP v3 auth sha <StrongPass> priv aes 128 <PrivKey>
ip access-list standard SNMP-MANAGER
 permit 10.10.100.50
 deny any
snmp-server group NMS-GRP v3 priv read SYSVIEW access SNMP-MANAGER
snmp-server contact it-team@bluewave.local
snmp-server location Building-1 MDF Rack-A
logging host 10.10.100.50
logging trap informational
service timestamps log datetime msec
service timestamps debug datetime msec
logging buffered 16384 informational
logging origin-id hostname
end
```
