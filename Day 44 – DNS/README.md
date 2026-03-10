
# **Day 44 – DNS (Domain Name System)**

> **Note:** This README contains **only explanations** — no configurations.  
> A simple DNS config file will be provided separately (Day 44 – configuration.md).

***

# 🧠 1. What Is DNS?

DNS is the **“phonebook of the internet.”**

Humans remember names:  
**google.com**, **youtube.com**, **cisco.com**

Computers only understand **IP addresses**:  
`142.250.190.14`  
`192.0.2.10`

DNS **translates** names into IPs automatically.

➡️ When you type a website name → DNS finds the correct IP.  
➡️ Without DNS → nothing loads, even if your internet works.

***

# 🏫 2. High‑School Analogy

Imagine trying to call your friend but only knowing their **phone number**, not their name.

DNS is like a **contact list**:

*   You type **“John”**
*   Phone shows **“+1 555 819 2020”**
*   You didn’t need to memorize the number

DNS does the same for websites.

***

# 🌐 3. Why DNS Is Important

Without DNS:

*   Browsers can't find websites
*   Emails cannot be delivered
*   Apps cannot reach servers
*   Wi‑Fi logins may fail
*   Networks break even if internet is “on”

DNS is a **core service** in every network.

***

# 🧩 4. Key DNS Concepts

### ✔ **DNS Resolver (Client)**

Your PC, phone, or router that asks the DNS question.

### ✔ **DNS Server**

The device that answers DNS questions.

Example:

> “What is the IP of google.com?”

### ✔ **A Record**

Maps a **name → IPv4 address**

Example:  
`google.com → 142.250.190.14`

### ✔ **AAAA Record**

Maps a **name → IPv6 address**

### ✔ **CNAME Record**

A nickname for another domain.

Example:  
`www.example.com` → `example.com`

### ✔ **DNS Cache**

Your device stores recent DNS answers to speed up future lookups.

***

# 🔍 5. How DNS Works (Super Simple)

1.  You type: **[www.google.com](http://www.google.com)**
2.  Your device sends a DNS query to a DNS server
3.  DNS server replies with the IP address
4.  Your browser connects to that IP
5.  Website loads

The entire process takes **milliseconds**.

***

# 📦 6. Public DNS Servers (Common Ones)

*   **8.8.8.8** → Google DNS
*   **1.1.1.1** → Cloudflare DNS
*   **9.9.9.9** → Quad9 (security filtering)

Routers often distribute DNS server info using **DHCP** (Day 37).

***

# 🧪 7. Packet Tracer Note (Important)

Cisco Packet Tracer:

*   ✔ Supports basic DNS *simulation*
*   ❌ Does NOT support full DNS server features
*   ✔ You can configure a simple DNS server for name → IP mapping
*   ✔ You can test DNS with the **ping** or **web browser tool**

DNS labs in Packet Tracer are **basic only**, but enough for CCNA.

***

# 📘 8. Reminder

This README contains **only explanations**.  
All configuration commands (simple DNS server + testing) will be included in:

➡ **Day 44 – DNS – configuration.md**
