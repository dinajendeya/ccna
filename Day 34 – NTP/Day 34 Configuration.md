# Day 34 – NTP (Network Time Protocol) – Configuration.md
*CCNA NTP configuration examples: client mode, server mode, master mode, authentication, and verification.*

---
# 📘 1) Scenario Overview
BlueWave University routers must synchronize their clocks using NTP.

Network Setup:
- **R1** → Acts as NTP client (syncs to ISP time server)
- **R2** → Acts as NTP server for internal devices
- **R3** → Acts as NTP client synchronizing from R2

ISP Time Server (Public NTP): `129.6.15.28` (example)
Internal Time Server (R2): `2001:DB8:20::1` or `192.168.20.1`

> Note: Replace the public NTP server with a real reachable one in your lab.

---
# 🛠 2) Basic NTP Client Configuration (R1)
R1 synchronizes time with a public NTP server.

```bash
configure terminal
ntp server 129.6.15.28
end
```

(Optional) Set timezone:
```bash
clock timezone EET 2
```

---
# 🛠 3) Configure R2 as an NTP Server
R2 provides NTP service to LAN devices.

```bash
configure terminal
ntp server 129.6.15.28   ! R2 syncs from ISP NTP
ntp master 5              ! R2 acts as internal source (stratum 5)
end
```

> If R2 has Internet access, `ntp master` is optional.

---
# 🛠 4) Configure R3 as NTP Client (Sync from R2)
```bash
configure terminal
ntp server 192.168.20.1   ! R2 internal IP
end
```

If using IPv6:
```bash
ntp server 2001:DB8:20::1
```

---
# 🛠 5) NTP Authentication (Optional but Recommended)
Use MD5 authentication for secure time updates.

### Step 1: Create authentication key (on all routers)
```bash
configure terminal
ntp authenticate
ntp authentication-key 1 md5 BLUEWAVE2026
ntp trusted-key 1
end
```

### Step 2: Apply key to NTP server statements
#### On R1:
```bash
ntp server 129.6.15.28 key 1
```

#### On R3 (syncing from R2):
```bash
ntp server 192.168.20.1 key 1
```

---
# 🛠 6) NTP Access Control (Optional)
Restrict who can query or sync.

```bash
configure terminal
access-list 10 permit 192.168.20.0 0.0.0.255
ntp access-group peer 10
ntp access-group serve 10
ntp access-group serve-only 10
end
```

---
# 🧪 7) Verification Commands
### Check NTP status
```bash
show ntp status
```
Expected:
```
Clock is synchronized
Reference is 129.6.15.28
```

### Check NTP associations
```bash
show ntp associations
```
Example output:
```
*~129.6.15.28      1   16     64      23.5ms
```
(*) means synchronized

### Show current time
```bash
show clock detail
```
Shows timezone, synced source, and drift info.

---

Common fixes:
- Check reachability with `ping`
- Ensure UDP port 123 allowed
- Verify authentication keys
- Use `ntp sync` after clearing configs

---
# 🏁 8) Final Summary
This configuration includes:
- NTP client (R1)
- NTP internal server/master (R2)
- NTP client using internal server (R3)
- IPv6 NTP support
- Authentication
- Access control
- Verification and troubleshooting

This completes **Day 34 – NTP – Configuration.md**.
