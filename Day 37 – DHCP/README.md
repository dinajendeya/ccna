# Day 37 – DHCP (Dynamic Host Configuration Protocol)
> **Note:** No configuration commands appear in this README.  
> All DHCP configurations will be provided separately in the `configuration.md` file for Day 37.  
> Day 38 will focus on protecting networks using **DHCP Snooping**.

---

# 🧠 1. The Story: Why Was DHCP Invented?
Imagine your school has hundreds of students. Each morning, every student must:
- Pick a locker
- Get a locker number
- Get the key
- Know which hallway their classroom is in
- Learn the school Wi‑Fi password

Now imagine the principal has to assign all of this **manually** every day. Students would be waiting in huge lines, mistakes would happen, and chaos would fill the hallways.

Early computer networks had the **exact same problem**.
Every device (PC, laptop, phone, printer) needed:
- An IP address
- A subnet mask
- A default gateway
- DNS servers
- Other network settings

Before DHCP, every one of these had to be typed in **by hand**.

This caused:
- Devices getting **duplicate IP addresses**
- People unable to connect
- Hours of troubleshooting
- Total disasters when networks got bigger

Engineers said:

➡️ *"We need a system that gives devices their network information automatically, just like a system that assigns locker numbers automatically."*

💡 That system became **DHCP**.

---

# 🌐 2. What Is DHCP (Explained Like You're in High School)?
**DHCP** is a protocol that automatically gives devices the information they need to join a network.

Think of DHCP as the **school front office**:
- When a student enters, the office gives them everything they need.
- When a device connects, DHCP gives it everything it needs.

DHCP provides:
- A unique IP address
- Subnet mask
- Default gateway
- DNS servers
- Extra options

All automatically.

No waiting in line. No manual typing. No mistakes.

---

# 🏫 3. High‑School Analogy
Imagine students arriving at school.

1. A student walks in and shouts: 
   **"I need a locker!"**  
   (DHCP Discover)

2. The office answers: 
   **"Here are some lockers available!"**  
   (DHCP Offer)

3. The student chooses one: 
   **"I want locker #15."**  
   (DHCP Request)

4. The office confirms: 
   **"Locker #15 is yours!"**  
   (DHCP Ack)

This is exactly how devices get their network settings.

---

# 🧩 4. The Four Steps of DHCP (The DORA Process)
DHCP uses four simple steps called **DORA**:

### 1️⃣ Discover — "Anyone there who can give me an IP?"
A device broadcasts a message to the whole network.

### 2️⃣ Offer — "Here’s an IP address you can use."
The DHCP server replies with an available IP.

### 3️⃣ Request — "I’d like to use that IP please."
The device accepts the offer.

### 4️⃣ Acknowledge — "Okay, it’s officially yours."
The server confirms and leases the IP.

After this, the device is ready to communicate.

---

# 🏆 5. Key Ideas Behind DHCP
Here are the main pieces of the DHCP system:

### ✔ IP Pool (Scope)
A range of IP addresses the server can give out.

### ✔ Lease
The length of time a device can use its assigned IP.
(Just like borrowing a book from the library.)

### ✔ Options
Extra information like:
- DNS servers
- Domain name
- Time server
- TFTP server

### ✔ Exclusions
Special IPs the server is **not** allowed to give out.
(E.g., those used by servers or routers.)

### ✔ Reservations
A device always gets the same IP.
(Great for printers or cameras.)

---

# 🔄 6. Step‑By‑Step: What Happens When a Device Connects?
Here’s the full flow explained simply:

1. The device wakes up and has **no IP address** yet.
2. It sends a **Discover** message asking for help.
3. The server looks for an available IP.
4. The server sends an **Offer**.
5. The device says **Request**.
6. The server replies with an **Ack**.
7. The device configures itself.
8. After some time, it **renews** the lease.

If the device leaves, the server gives the IP to someone else.

---

# 📡 7. Important DHCP Messages
DHCP uses several message types:
- **Discover** – Find a server
- **Offer** – Server proposes settings
- **Request** – Client accepts them
- **Acknowledge (ACK)** – Final confirmation
- **Decline** – Client refuses because IP is already used
- **Release** – Client hands back its IP
- **Inform** – Client asks for extra info only

---

# ⏱ 8. Leases and Timers (Explained Simply)
Think of a lease as a temporary permission.

If the lease lasts **24 hours**:
- At **12 hours (50%)**, the device tries to renew.
- At **21 hours (87.5%)**, it tries again.
- If renewal fails, the device must start over.

This prevents old devices from keeping IPs forever.

---

# 🏗 9. Real‑World Example
A university has many VLANs:
- Staff
- Students
- VoIP phones
- Wi‑Fi

Instead of running a DHCP server in every building, they keep **one central DHCP server**.

Each router interface has a **DHCP relay** that acts like a mailman:
- It carries DHCP messages from clients to the server.
- It brings back the reply.

The result:
- All devices get IPs automatically.
- One server manages everything.
- No messy manual configuration.
- Faster setup for thousands of students.

---

# 🔒 10. Security Preview (Leads Into Day 38)
DHCP can be attacked:
- **Rogue DHCP Server:** A fake server gives wrong gateways.
- **Starvation Attack:** An attacker requests thousands of IPs and empties the pool.

To fix this, switches use:
- **DHCP Snooping** (Day 38)
- **Trusted/Untrusted ports**
- **Binding tables**

You’ll learn all about this tomorrow.

---

# 🌱 11. Quick Look at DHCP for IPv6
IPv6 adds more options:
- **SLAAC** (devices create their own IPs)
- **Stateless DHCPv6** (only gives extra info)
- **Stateful DHCPv6** (works like IPv4 DHCP)
- **Prefix Delegation** (routers receive entire IPv6 blocks)

But the idea stays the same:
➡️ make IP addressing automatic.

---

# 🔗 12. Relationship With Next Days
- **Day 38:** Protecting DHCP using DHCP Snooping

---

# 📘 13. Reminder
This README contains **only explanations**.  
All DHCP configuration commands are included in:

➡ **Day 37 – DHCP – configuration.md**
