
# **Day 40 – Wireless LANs (Wi‑Fi Basics)**

> **Note:** No configuration commands appear in this README.  
> All wireless configurations will be provided separately in:  
> ➜ **Day 40 – Wireless LANs – configuration.md**
>
> Day 41 will move on to **WAN Technologies**, showing how remote buildings connect together.

***

# 🧠 1. The Story: Why Wireless LANs Were Invented

Imagine a school building where students bring laptops, tablets, and phones.  
If every device needed a **network cable**, the place would be a jungle of wires:

*   Cables across hallways
*   No flexibility to move
*   Devices stuck in one spot
*   Fire hazards
*   Huge installation costs

Engineers realized:

➡️ “We need networking **without cables**—something flexible, mobile, and still fast.”

That became **Wireless LANs (WLANs)**, commonly known as **Wi‑Fi**.

Wi‑Fi lets devices connect through the **air** using radio waves instead of cables.

***

# 🌐 2. What Is a Wireless LAN?

A **Wireless LAN (WLAN)** is a network where devices connect using **radio signals** to access the same LAN that wired devices use.

In simple terms:

📡 **Access Point (AP)** = The Wi‑Fi “radio tower”  
🧑‍🤝‍🧑 **Clients** = Laptops, phones, printers  
🌐 **SSID** = The Wi‑Fi network name  
🔑 **Authentication** = How devices join (password, 802.1X, etc.)  
🔌 **Wired backbone** = Where APs ultimately connect

***

# 🧩 3. Components of a Wireless LAN

### ✔ Access Point (AP)

The device that sends and receives Wi‑Fi signals.  
APs connect to the **wired network** using Ethernet and often get power via PoE.

### ✔ SSID (Wi‑Fi Network Name)

Examples:

*   *BlueWave‑Staff*
*   *BlueWave‑Students*
*   *BlueWave‑Guest*

Each SSID typically maps to its own **VLAN**.

### ✔ Wireless Controller (WLC)

Large networks use a **WLC** to manage many access points at once:

*   Pushes configurations
*   Manages SSIDs
*   Handles authentication
*   Performs roaming decisions
*   Applies security policies

### ✔ Clients

Phones, laptops, printers, IoT devices—anything with Wi‑Fi.

***

# 📡 4. How Wi‑Fi Communication Works (Simple Explanation)

1.  AP broadcasts the SSID.
2.  Devices nearby detect it.
3.  A device sends a **join request**.
4.  AP allows it if the password/security is valid.
5.  Device gets an **IP address** (using DHCP from previous days).
6.  Device starts sending data through the air.
7.  AP passes that data onto the wired network.

Everything looks “wireless” to the user,  
but behind the scenes **APs are wired**.

***

# 🛑 5. Channels, Interference, and Why Wi‑Fi Gets Slow

Wi‑Fi uses shared radio channels.  
Problems happen when:

*   Too many APs use the same channel
*   Nearby networks overlap
*   Microwave ovens / Bluetooth devices interfere
*   Rooms have thick walls

Result → slow or unstable Wi‑Fi.

This is why:

✔ APs must use different channels  
✔ Power levels must be controlled  
✔ 2.4 GHz and 5 GHz bands are used differently

***

# 🔐 6. Wi‑Fi Security (Why It Matters)

Wireless is easier to attack because signals travel through the air.  
Common security standards:

### ❌ WEP (Not secure)

### ✔ WPA2 (Standard for many years)

### ✔ WPA3 (Modern and stronger)

Secure Wi‑Fi prevents:

*   Unauthorized access
*   Data theft
*   MITM attacks
*   Rogue AP connections

***

# 📘 7. Reminder

This README contains **only explanation**.  
All wireless configurations will be provided in:

➡ **Day 40 – Wireless LANs – configuration.md**

