# Windows Server 2012 Setup: Step-by-Step Guide

This guide walks through configuring a **Windows Server 2012 machine** in a VirtualBox lab environment. The server hosts a vulnerable version of **Rejetto HFS 2.3m**, allowing attackers to exploit a **Remote Command Execution (RCE)** vulnerability and gain **Administrator access**.

---

##  Prerequisites

- VirtualBox installed
- Windows Server 2012 ISO (Standard/Datacenter)
- Internet access for the VM (via NAT network or host-only, as needed)
- Lab network: `192.168.2.0/24` (Active Directory network)

---

## 1️ Windows Server 2012 Installation

1. **Create a new VM in VirtualBox**:
   - Name: `WinServer2012`
   - Type: Windows
   - Version: Windows 2012 (64-bit)
   - RAM: 2 GB (minimum)
   - HDD: 40 GB (or more)

2. **Attach to the Lab Network**:
   - Adapter 1: **NAT Network** (e.g., `ADNet`, 192.168.2.0/24)

3. Boot from the Windows Server 2012 ISO and complete the installation:
   - Follow standard installation prompts.
   - Set Administrator password.

4. After installation:
   - Set static IP in the `192.168.2.0/24` network:
     - Example:
       - IP: `192.168.2.50`
       - Subnet: `255.255.255.0`
       - Gateway: `192.168.2.1`
   - Disable firewall for testing (optional in lab only):
     - Open PowerShell as Administrator:
       ```powershell
       Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
       ```

---

## 2️ Install Rejetto HFS 2.3m

1. Download Rejetto HFS 2.3m:
   - URL: https://sourceforge.net/projects/hfs/files/HFS/2.3m/

2. Extract and run HFS:
   - No installation required — it runs as a standalone executable.
   - Place the `hfs.exe` somewhere easy to find (e.g., Desktop or C:\Tools).

3. Run `hfs.exe`:
   - Right-click → **Run as Administrator** (recommended).
   - Allow through Windows Defender if prompted.

4. Configure HFS:
   - Click **Menu > IP Address**: Set to `0.0.0.0` (listen on all interfaces).
   - Click **Menu > Port**: Set to `80` or another desired port.
   - Enable file sharing by dragging a folder into the HFS window (optional for lab).
   - Confirm HFS is accessible:
     - Visit `http://<Win2012-IP>:80` in a browser.

---

## 3️ Vulnerable Service Ready

The Windows Server 2012 machine is now running **Rejetto HFS 2.3m**, which is known to be vulnerable to **Remote Command Execution (RCE)** via specially crafted HTTP requests.

---


