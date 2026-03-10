

## Day 04 – Intro to the CLI

### What Is a CLI?
- **Command‑Line Interface (CLI):** the interface used to configure and operate Cisco devices.
- **GUI (Graphical User Interface):** visual interface; most Cisco network configuration is performed in the CLI.

---

### Connecting to a Cisco Device
- **Console port:** used for first‑time configuration and out‑of‑band access.
- **Rollover (console) cable:** DB9‑to‑RJ‑45 or DB9‑to‑USB, depending on the PC.

**Accessing the CLI**
- Use a **terminal emulator** (e.g., PuTTY) and connect via **Serial** with default settings.
- However Packet Tracer enable us to configure routers easialy in CLI section

```bash
# Example: PuTTY Serial session
# Serial line: COM3         # (Select your actual COM port)
# Speed:      9600
# Data bits:  8
# Stop bits:  1
# Parity:     None
# Flow control: None
```

---

### CLI Modes and Prompts

**User EXEC Mode**
- Prompt: `(hostname)>`
- View‑only, very limited; cannot modify configuration.
- Often called **User mode**.
- Enter `enable` to switch to Privileged EXEC.

**Privileged EXEC Mode**
- Prompt: `(hostname)#`
- Full visibility and operational control (view configuration, save, reload, set time), **but no feature/interface configuration** until you enter configuration mode.

**Navigation & Help**
- **`?`** shows available commands in the current mode; with partial input, lists matching commands.
- **TAB** auto‑completes valid, unambiguous commands.

---

### Global Configuration Mode
- From Privileged EXEC, enter **Global Configuration**:
```bash
enable
configure terminal
# or: conf t
```
- Prompt changes to `(config)#`.
- Use `exit` to return to Privileged EXEC.

---

### Privileged Passwords & Encryption

**Enable Password (legacy/plaintext unless encrypted)**
```bash
enable
configure terminal
enable password <PASSWORD>
```
- Case‑sensitive.
- Stored as plaintext unless global password encryption is enabled.

**Global Plaintext Password Obfuscation**
```bash
enable
configure terminal
service password-encryption
```
- Encrypts **current** plaintext‑type passwords and **all future** plaintext‑type passwords.
- Does **not** affect `enable secret` (already hashed).
- If later disabled:
  - Existing encrypted passwords remain encrypted (not decrypted).
  - Future plaintext‑type passwords will not be encrypted.
  - `enable secret` remains unaffected.

**Enable Secret (preferred/hashed; overrides enable password)**
```bash
enable
configure terminal
enable secret <PASSWORD>
```
- Always stored as a stronger hash (commonly referred to as type 5/MD5‑style).
- **Overrides** any configured `enable password` during authentication.

---

### Running vs Startup Configuration

**Definitions**
- **running‑config (RAM):** the live, active configuration; commands apply immediately.
- **startup‑config (NVRAM):** the configuration loaded when the device restarts.

**Viewing Configurations**
```bash
show running-config
show startup-config
```

**Saving the Running Configuration**
```bash
write
write memory
copy running-config startup-config
# Destination filename [startup-config]?  <Press Enter>
```

---

### Encrypting Passwords — Practical Flow
```bash
enable
configure terminal
service password-encryption
# Result:
# - Current plaintext-type passwords are encrypted.
# - Future plaintext-type passwords will be encrypted.
# - 'enable secret' is independent and already hashed.
```

- **Type “7”**: legacy, reversible Cisco encryption applied to plaintext‑type passwords when `service password-encryption` is active; weak and easily cracked.
- **Type “5”**: MD5‑style hashed secret used by `enable secret`; significantly stronger than type 7 (still theoretically crackable).
- `enable secret` **overrides** `enable password` if both exist.

---

### Negating / Deleting Configuration
- Prepend **`no`** to the command you want to remove/disable.
```bash
enable
configure terminal
no service password-encryption
```
- Results of disabling password encryption:
  - Existing encrypted passwords **remain** encrypted (not decrypted).
  - Future plaintext‑type passwords are **not** encrypted.
  - `enable secret` remains unaffected.
