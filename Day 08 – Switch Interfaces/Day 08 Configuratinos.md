
# CCNA Configurations — Day 08 (Essential Switch Interfaces)

## 1) Show IP Interface Summary
**Why:** Quickly verify IPv4 addressing and interface operational status.  
**Where:** Privileged EXEC mode.

```bash
Switch# show ip interface brief
```

**Result:** Displays interface names, IP addresses, assigned method, status, and protocol.

---

## 2) Show Interface Status (Switchports)
**Why:** Check VLAN assignment, duplex, speed, and interface state.  
**Where:** Privileged EXEC.

```bash
Switch# show interfaces status
```

**Result:** Summary of switchport operational parameters such as VLAN, speed, duplex, and link status.

---

## 3) Detailed Interface Information
**Why:** Troubleshoot physical issues, errors, duplex mismatches, speed, and utilization.  
**Where:** Privileged EXEC.

```bash
Switch# show interfaces
```

**Result:** Full detailed output including:
- Speed and duplex
- MAC counters
- Errors (CRC, collisions)
- Input/output rates

---

## 4) Configure Multiple Interfaces (Interface Range)
**Why:** Apply configurations to multiple switchports at once.  
**Where:** Global configuration mode.

```bash
Switch# conf t
Switch(config)# interface range fastEthernet0/x - y
```

**Result:** Interface prompt changes to `Switch(config-if-range)#` allowing bulk configuration.

---

## 5) Shutdown Interface(s)
**Why:** Administratively disable ports for security or maintenance.  
**Where:** Interface or interface-range configuration mode.

```bash
Switch(config-if)# shutdown
```

**Result:** Interface status becomes **administratively down**.

---

## 6) Enable Interface(s) — no shutdown
**Why:** Bring an interface back online.  
**Where:** Interface or interface-range configuration mode.

```bash
Switch(config-if)# no shutdown
```

**Result:** Interface transitions to **up** if a physical connection exists.

