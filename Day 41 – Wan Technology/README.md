# **Day 41 – WAN Technologies**

> **Note:** WAN *configuration* is **not required** in CCNA.  
> This README gives only the **basic concepts**, in the same style as previous days.

***

# 🧠 1. What Is a WAN?

A **WAN (Wide Area Network)** connects networks that are **far apart**:

*   Different buildings
*   Different campuses
*   Different cities

Think of it as a giant road system that connects places that are too far for regular cables.

***

# 🏫 2. univirsity Analogy

Your univirsity has:

*   Main building
*   Sports complex
*   Administrative office across the city

Each has its own local network (LAN).  
A **WAN** is like the **univirsity bus system** that connects all campuses.

LAN = inside building  
WAN = between buildings

***

# 🌐 3. Why Do We Need WANs?

Because:

*   Companies have multiple branches
*   univirsities have several campuses
*   Remote workers need access to office resources
*   Cloud services must be reachable

Without WANs, each location would be isolated.

***

# 🧩 4. Basic WAN Types (Simple Overview)

### ✔ MPLS (Modern Business WAN)

*   Provided by telecom companies
*   Private, reliable connections between sites
*   VERY common in schools, banks, companies

### ✔ Site‑to‑Site VPN

*   Secure tunnel over the **internet**
*   Used by small offices and home workers
*   Encrypts all traffic

### ✔ Dedicated Internet / Fiber

*   High‑speed connections for modern WANs
*   Often used together with VPNs

### ✔ 4G / 5G WAN

*   Backup link for emergencies
*   Works anywhere with cellular coverage

### ✔ PPP / Serial Links (Legacy CCNA Topic)

*   Old point‑to‑point circuits
*   Used mostly for teaching history of WANs

***

# 🚦 5. WAN vs LAN (Super Simple)

| Feature  | LAN              | WAN                  |
| -------- | ---------------- | -------------------- |
| Distance | Building         | City or beyond       |
| Speed    | Fast (1–10 Gbps) | Slower (10–500 Mbps) |
| Cost     | Cheap            | Expensive            |
| Owner    | You              | Telecom provider     |

WANs cost more because you are using the provider’s network.

***

# 🔐 6. WAN Security (Why It Matters)

Your data travels through:

*   Telecom networks
*   Public internet (for VPNs)
*   Shared infrastructure

So WANs must use:

✔ Encryption  
✔ Authentication  
✔ Access control  
✔ Monitoring (SNMP – tomorrow’s topic)

***

# 🏫 7. Real‑World Example (One Scenario Only)

BlueWave School has:

*   Main campus
*   Two smaller campuses in different districts

They use:

*   **MPLS WAN** from a telecom provider to link campuses
*   **VPN** for remote teachers working from home
*   **Fiber internet** for cloud services

From a user perspective, everything feels like one big network — even though the buildings are miles apart.

***

# 📘 8. What CCNA Actually Wants You to Know

You **do NOT** need to configure WANs.  
You only need to understand:

*   What a WAN is
*   Why WANs exist
*   Basic WAN types (MPLS, VPN, broadband, cellular)
*   PPP basics
*   Role of service providers

That’s all.

***
