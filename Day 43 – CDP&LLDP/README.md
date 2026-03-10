
# **Day 43 – CDP & LLDP (Neighbor Discovery Protocols)**

> **Note:** This README contains **only explanations** — no configuration commands.  
> All configuration commands will be provided separately in:  
> ➜ **Day 43 – CDP & LLDP – configuration.md**
>
> CDP and LLDP are very small CCNA topics, but extremely useful when troubleshooting.

***

# 🧠 1. Why Do We Need CDP & LLDP?

Imagine you walk into a school network room full of switches, routers, and cables.  
Nothing is labeled.  
You don’t know:

*   What device is connected to which port
*   What the neighbor device’s name is
*   What model or operating system it’s running
*   What interface connects to what

This is a nightmare when troubleshooting.

Engineers needed a way for network devices to **introduce themselves** to each other automatically.

So we got:

*   **CDP** → Cisco Discovery Protocol
*   **LLDP** → Link Layer Discovery Protocol (vendor‑neutral)

Both help you map out the network **without guessing**.

***

# 🌐 2. What Is CDP?

**CDP (Cisco Discovery Protocol)** is a Cisco‑proprietary protocol.  
It works on Layer 2 (Data Link Layer) and lets Cisco devices share basic information about themselves.

CDP tells you:

*   Device name
*   Device model
*   IP address
*   Interface used
*   VLAN info
*   IOS version
*   Capabilities (router, switch, phone, etc.)

It’s like Cisco devices saying:

➡️ “Hi, I’m Switch1 on port G0/1. Nice to meet you.”

CDP works only between Cisco devices.

***

# 🌐 3. What Is LLDP?

**LLDP (Link Layer Discovery Protocol)** is the **industry standard version** of CDP.

*   Works on Cisco
*   Works on HP
*   Works on Juniper
*   Works on VoIP phones
*   Works on anything that follows IEEE standards

LLDP is used in **multi‑vendor networks**, so devices from different companies can recognize each other.

Think of LLDP as:

➡️ “A universal language for devices to identify themselves.”

***

# 🧩 4. What Information Do They Share?

Both CDP and LLDP send small messages that include:

*   Device name
*   Port number
*   Neighbor’s port number
*   Device type
*   IP address
*   VLAN / capabilities

This helps engineers trace connections like:

    Switch1 (G0/1) ---> (F0/5) Switch2

Or even:

    Switch ---> AP
    Switch ---> IP Phone ---> PC

Very helpful in troubleshooting voice networks and wireless networks.

***

# 🔍 5. Why Are CDP & LLDP Useful?

### ✔ Troubleshooting

When a link is down or incorrectly wired, CDP/LLDP show you exactly which devices are on the other side.

### ✔ Network Documentation

Helps map the entire physical network automatically.


### ✔ Multi‑Vendor Support

LLDP is crucial when devices come from different companies.

***

# 🛑 6. Security Considerations

CDP and LLDP reveal device information.  
Attackers could use this to plan attacks.

So best practice:

*   Enable CDP/LLDP **only on internal/trusted interfaces**
*   Disable it facing the **internet or external networks**

***

# 🧪 7. About Packet Tracer

Packet Tracer **does support CDP**, but **LLDP is very limited**.

*   CDP neighbor information works normally
*   LLDP support depends on the device image
*   Some features (like LLDP‑MED for phones) are not available

So you can practice **CDP fully**, but LLDP will be **partially conceptual**.

***

# 🏫 8. Real‑World Example

You plug a cable into a switch and wonder:

➡️ “What is on the other end?”

CDP/LLDP can instantly tell you:

*   It's connected to a Cisco 2960 on port G0/24
*   Device name is *SW-LAB2*
*   It runs IOS version X.Y
*   It belongs to VLAN 20

This saves **hours** of tracing cables.

***

# 📘 9. Reminder

This README includes **only explanations**.  
All configuration commands (CDP enable/disable, LLDP basics) will be included in:

➡ **Day 43 – CDP & LLDP – configuration.md**
