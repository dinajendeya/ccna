# CCNA Configurations — Days 05 (Ethernet LAN Switching)

## 1) Inspect the ARP Table
**Why:** Verify L3→L2 address resolution entries (IP ↔ MAC) on a host or L3 switch/router.  
**Where:** Privileged EXEC.

```bash
show arp
```
**Result (example snippet):**
```bash
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.0.2.10              1   aaaa.bbbb.cc01  ARPA   Vlan10
Internet  192.0.2.20              -   aaaa.bbbb.cc02  ARPA   Vlan10
Internet  198.51.100.5            3   aaaa.bbbb.cc05  ARPA   GigabitEthernet0/1
```

---

## 2) Trigger ARP Resolution With Ping
**Why:** Force the device to learn/refresh the MAC for a known IP; validates L3 reachability and ARP learning.  
**Where:** Privileged EXEC.

```bash
ping 192.0.2.10
```
**Result (typical indicators):**
```bash
!!!!!        # 5 successful replies (default count)
# Dots (.) indicate failures, exclamation (!) indicates success.
```

---

## 3) View the Switch MAC Address Table
**Why:** Confirm L2 learning and forwarding decisions (VLAN, MAC, type, interface).  
**Where:** Privileged EXEC on a switch.

```bash
show mac address-table
```
**Result (example snippet):**
```bash
          Mac Address Table
-------------------------------------------
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
 10     aaaa.bbbb.cc01    DYNAMIC     Gi0/1
 10     aaaa.bbbb.cc02    DYNAMIC     Gi0/2
 20     aaaa.bbbb.cc10    DYNAMIC     Gi0/3
 1      aaaa.bbbb.ccff    STATIC      CPU
```

---

## 4) Clear Dynamic MAC Entries (All)
**Why:** Troubleshoot stale/incorrect L2 entries; force fresh learning.  
**Where:** Privileged EXEC on a switch.

```bash
clear mac address-table dynamic
```
**Result:** Dynamic entries removed; table repopulates as frames arrive.

---

## 5) Clear Dynamic MAC Entries on a Specific Interface
**Why:** Targeted cleanup when only one port’s learning is suspect.  
**Where:** Privileged EXEC on a switch.

```bash
clear mac address-table dynamic interface <INTERFACE>
# example
clear mac address-table dynamic interface f0/1
```
**Result:** Dynamic MACs learned on the specified interface are removed; new traffic relearns them.

---

## 6) Verify Minimum Ping Behavior (Defaults)
**Why:** Understand default probe count and success/failure markers when validating links.  
**Where:** Privileged EXEC.

```bash
ping 198.51.100.5
```
**Result (indicative):**
```bash
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 198.51.100.5, timeout is 2 seconds:
!.!..
Success rate is 60 percent (3/5), round-trip min/avg/max = 2/4/9 ms
```

---

## 7) ARP Sanity Check After Ping
**Why:** Confirm the ARP cache was populated by the ping operation.  
**Where:** Privileged EXEC.

```bash
show arp | include 192.0.2.10
```
**Result (example):**
```bash
Internet  192.0.2.10   0   aaaa.bbbb.cc01  ARPA  Vlan10
```

---

## 8) Understand MAC Aging (Operational Note)
**Why:** MAC entries are removed after inactivity; awareness prevents misinterpretation during troubleshooting.  
**Where:** Switch data plane (automatic behavior).

```bash
# No command needed to age out; default dynamic aging is ~5 minutes (platform dependent).
# Use show commands to observe disappearance/relearning during idle/traffic periods.
show mac address-table | include aaaa.bbbb
```
**Result:** Entries vanish after the aging timer if no frames are seen; reappear on next valid frame from the source.
