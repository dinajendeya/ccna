
# Day 38 – DHCP Snooping

## 🧠 1. Why DHCP Snooping Exists

In a network, devices get IP addresses from a DHCP server.  
But if someone plugs in a **fake DHCP server**, they can hand out:

*   Wrong IP addresses
*   Fake gateways (to intercept traffic)
*   Fake DNS servers (to redirect websites)

This breaks the network and creates big security risks.

💡 **DHCP Snooping** was invented to stop this.

***

## 🌐 2. What DHCP Snooping Does

DHCP Snooping protects the network by:

1.  **Watching all DHCP traffic** on the switch
2.  **Blocking fake DHCP servers** on user ports
3.  **Allowing only trusted ports** to send server replies
4.  **Building a binding table** (MAC–IP–VLAN–Port) for security

Think of it as a **hall monitor** ensuring only the real front office can assign lockers.

***

## 🏫 3. High‑School Analogy

*   Front office = Real DHCP server
*   Hall monitors = Trusted uplink ports
*   Students = Devices connecting
*   Random student pretending to assign lockers = Rogue DHCP server

DHCP Snooping checks **who is allowed** to give “locker numbers” (IP addresses) and blocks everyone else.

***

## 🔌 4. Trusted vs Untrusted Ports

*   **Trusted ports** → Uplinks to distribution/core where the real DHCP server is
*   **Untrusted ports** → Access ports where users plug in devices

Only **trusted ports** can send DHCP Offers/ACKs.  
Untrusted ports cannot behave like a DHCP server.

***

## 📘 5. Binding Table

The switch records:

*   MAC address
*   IP address
*   VLAN
*   Port
*   Lease time

This table helps prevent future attacks like ARP spoofing.

***

## 🛡 6. What DHCP Snooping Prevents

*   Rogue DHCP servers
*   DHCP starvation attacks
*   Fake DNS/gateway assignments
*   Man‑in‑the‑middle attacks
*   Network outages caused by wrong IP info

***

## 🏗 7. Real‑World Example (BlueWave University)

At a large campus:

*   Thousands of students
*   Many access switches
*   Public areas where anyone can plug in a device

DHCP Snooping ensures:

*   Only the real DHCP server can assign IPs
*   Rogue devices cannot break the network
*   Binding tables support extra security features

Result → **Stable, safe, predictable IP assignments**.

***

## 📌 9. Reminder

This README explains the concept only.  
All configuration commands are provided separately in:

➡ **Day 38 – DHCP Snooping – configuration.md**
