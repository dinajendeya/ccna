# **Day 50 – Ansible / Puppet / Chef (Automation Tools Overview)**

> **Note:** CCNA does **NOT** require configuration of automation tools.  
> This README provides **only simple explanations** of Ansible, Puppet, and Chef.
>
> Packet Tracer does **not support** these tools, so labs cannot be performed here.

***

# 🧠 1. Why Do Automation Tools Exist?

As networks grow, configuring devices manually becomes slow and full of mistakes:

*   Updating 30 switches → takes hours
*   Changing passwords everywhere → boring and risky
*   Fixing misconfigurations → time‑consuming

Automation tools solve this problem by allowing you to:

*   Configure hundreds of devices at once
*   Keep configurations consistent
*   Reduce human error
*   Save time

Tools like **Ansible, Puppet, and Chef** help engineers manage large networks more easily.

***

# 🏫 2. High‑School Analogy

Imagine a teacher needs to write a message on **30 classroom boards**.

Without automation → walk to each room and write it manually.  
With automation → press one button and the message appears in every classroom instantly.

That’s what automation tools do for network devices.

***

# 🌐 3. What Is Ansible?

**Ansible** is the simplest and most popular automation tool for networking.

### ✔ Why it’s simple

*   No agent needed on devices
*   Uses SSH or APIs
*   Uses human‑readable **YAML** files (Day 48)

### ✔ What it does

*   Pushes configuration
*   Creates backups
*   Checks device status
*   Automates repetitive tasks

### ✔ Why CCNA mentions Ansible

Because it’s commonly used with Cisco IOS, IOS‑XE, and NX‑OS.

***

# 🌐 4. What Is Puppet?

**Puppet** is an automation tool mostly used in data centers and server environments.

### ✔ Key points

*   Uses “agents” on devices
*   Controller pushes rules (“desired state”)
*   More common in Linux/Cloud environments than networking

### ✔ Networking use

Puppet can configure network devices, but it is **less common** than Ansible in Cisco environments.

***

# 🌐 5. What Is Chef?

**Chef** is also a configuration‑management tool.

### ✔ Key points

*   Uses Ruby‑based “recipes”
*   Automates server and cloud configurations
*   Focuses more on DevOps than traditional networking

### ✔ Networking use

Chef supports network automation, but like Puppet, is not as popular for Cisco devices as Ansible.

***

# 🧩 6. Comparing the Three

| Feature                        | Ansible   | Puppet     | Chef    |
| ------------------------------ | --------- | ---------- | ------- |
| Popular for network automation | ⭐⭐⭐⭐⭐     | ⭐⭐         | ⭐       |
| Uses agent?                    | ❌ No      | ✔ Yes      | ✔ Yes   |
| Language                       | YAML      | Puppet DSL | Ruby    |
| Ease of use                    | Easy      | Medium     | Harder  |
| Cisco support                  | Excellent | Limited    | Limited |

CCNA takeaway:

➡️ **Ansible is the most relevant tool for network engineers.**

***

# 🤖 7. Why Network Engineers Use These Tools

Automation tools help with:

*   Deploying new switches automatically
*   Keeping all configs consistent
*   Updating firmware
*   Rolling out security changes quickly
*   Reducing manual typing
*   Preventing misconfigurations
*   Managing large networks

This is extremely important in modern enterprise networks.

***

# 🧪 8. Packet Tracer Note (Important)

Packet Tracer **does NOT support**:

*   Ansible
*   Puppet
*   Chef
*   API connections
*   SSH automation
*   Python automation

You can learn **concepts only**.

To practice in the real world, engineers use:

*   GNS3
*   EVE‑NG
*   Cisco Modeling Labs (CML)
*   Real equipment
*   Linux virtual machines running Ansible/Puppet/Chef

***

# 🏫 9. Real‑World Example (Simple)

BlueWave School buys 20 new switches.  
Instead of configuring them one by one:

*   The network team uses **Ansible**
*   Ansible pushes VLANs, passwords, and SNMP settings to all switches
*   Installation takes minutes instead of hours
*   All configs are identical and documented

This is why automation is now standard in modern networks.

***

# 🔗 10. How This Connects to Previous Days

*   **Day 47 (Network Automation)** → This is the next step: *using tools to automate*
*   **Day 48 (YAML/JSON/XML)** → Ansible uses YAML; APIs use JSON
*   **Day 49 (REST APIs)** → Automation tools communicate with devices using APIs
*   **Day 46 (SDN)** → Controllers use automation heavily behind the scenes

***

# 📘 11. Reminder

This README includes **only explanations**, because:

*   **Ansible / Puppet / Chef configuration is NOT required in CCNA**
*   Packet Tracer cannot simulate these tools
*   CCNA wants only the **conceptual understanding**
