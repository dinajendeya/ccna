
# **Day 49 – REST APIs (Basic CCNA Overview)**

> **Note:** This README contains **only explanations**.  
> CCNA requires **basic understanding only** — not coding or API configuration.  
> Day 50 will cover **Ansible**, which uses APIs and automation concepts.

***

# 🧠 1. What Is a REST API?

Modern network devices (new switches, routers, controllers) no longer rely only on CLI.  
They can also communicate using **REST APIs**.

A **REST API** allows software to:

*   Ask a device for information
*   Send configuration changes
*   Monitor status
*   Automate tasks

It’s like a **universal language** for software to talk to network devices.

REST = **Representational State Transfer**  
API = **Application Programming Interface**

Together → A way for programs to interact with network devices **over HTTP/HTTPS**.

***

# 🏫 2. High‑School Analogy

Imagine your school has a website:

*   `/students` → list all students
*   `/teachers` → list all teachers
*   `/schedule` → get today’s schedule

When your browser visits those links, it gets data.

REST APIs work the same way:

*   `/interfaces` → list network interfaces
*   `/vlans` → list VLANs
*   `/health` → show device status

Software uses these “URLs” to talk to the device.

***

# 🌐 3. Why Are REST APIs Important?

Network automation tools (Day 47) need a **reliable way** to communicate with devices.

REST APIs let tools:

*   Read device information
*   Add VLANs
*   Change settings
*   Check logs
*   Build dashboards
*   Manage a whole network from one place

This is why APIs are becoming **standard** in modern networks.

***

# 🧩 4. How REST APIs Work

REST APIs use four main methods:

| Method     | Meaning (Simple Explanation)      |
| ---------- | --------------------------------- |
| **GET**    | Ask for information (read only)   |
| **POST**   | Create something (add new config) |
| **PUT**    | Update something                  |
| **DELETE** | Remove something                  |

Example (concept only):

*   GET `/interfaces` → “Give me all interface info”
*   POST `/vlans` → “Create this VLAN”

CCNA does **not** require using these commands — only recognizing them.

***

# 📦 5. REST APIs Use JSON

When you call an API, the device often replies using **JSON** (from Day 48).

For example, the device might answer:

    {
      "hostname": "Switch1",
      "interfaces": 24,
      "status": "healthy"
    }

CCNA only expects you to **identify** this as JSON.

***

# 🔐 6. REST API Security (Basic)

REST APIs use:

*   **HTTPS** (encrypted)
*   **Tokens** or **API keys**
*   **User authentication**

CCNA-level takeaway:

➡️ APIs must be secured like any login method.

***

# 🧪 7. Packet Tracer Note

Packet Tracer **does NOT support real REST APIs**.  
You cannot:

*   Run API calls
*   Send GET/POST requests
*   Interact with real controllers

So Day 49 is **pure theory** in CCNA labs.

***

# 🏫 8. Real‑World Example

BlueWave School uses a network controller.  
An IT admin opens a dashboard and clicks:

*   “Add VLAN 40”
*   “Show all switches”
*   “Check health”

The controller is actually sending **REST API requests** behind the scenes.

No CLI needed.

***

# 📘 9. Reminder

This README includes **only explanations**.  
REST API configuration or coding is **outside CCNA scope**.
