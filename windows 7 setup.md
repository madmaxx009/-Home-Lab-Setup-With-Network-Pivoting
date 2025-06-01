# Windows 7 Setup: Step-by-Step Guide

This guide walks through configuring a **Windows 7** machine in a VirtualBox lab environment for penetration testing. The target will be vulnerable to **MS17-010 (EternalBlue)**, allowing you to exploit it via Metasploit and gain a SYSTEM-level shell.

---

##  Prerequisites

- VirtualBox installed
- Windows 7 ISO (x64 preferred for EternalBlue exploit)
- Lab network: `192.168.2.0/24` (AD network)
- Internal Ubuntu machine (pivot point)

---

## 1️ Windows 7 Installation

1. **Create a new VM in VirtualBox**:
   - Name: `Win7`
   - Type: Windows
   - Version: Windows 7 (64-bit)
   - RAM: 2 GB (minimum)
   - HDD: 40 GB (or more)

2. **Attach to Lab Network**:
   - Adapter 1: **NAT Network** (e.g., `ADNet`, 192.168.2.0/24)

3. Install Windows 7:
   - Follow standard installation prompts.
   - Set Administrator password (e.g., `P@ssw0rd`).

4. After installation:
   - Set static IP in the `192.168.2.0/24` network:
     - Example:
       - IP: `192.168.2.60`
       - Subnet: `255.255.255.0`
       - Gateway: `192.168.2.1`
   - (Optional for lab) Disable Windows Firewall:
     - Control Panel > System and Security > Windows Firewall > Turn off Firewall.

---

## 2️ Prepare Windows 7 for MS17-010 Vulnerability

### Confirm Windows 7 is **unpatched** for MS17-010

MS17-010 affects:
- Windows 7 SP1 (x64 and x86)
- Ensure you **do NOT install the patch KB4012212 or KB4012215**.

Verify patch status:
- Open `Control Panel > Windows Update > View Update History`.
- Confirm **KB4012212/KB4012215** is **not installed**.
- If installed, uninstall the patch:
  - `Control Panel > Programs and Features > Installed Updates`.
  - Locate and remove any relevant KB4012212/KB4012215.

---

