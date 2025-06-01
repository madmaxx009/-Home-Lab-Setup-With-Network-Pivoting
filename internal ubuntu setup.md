# Internal Ubuntu Server Setup: Step-by-Step Guide

This guide explains how to configure the **Internal Ubuntu Server** with dual network interfaces, SSH access (with a weak password for brute-force), a misconfigured `find` command for privilege escalation, and installation of **Nmap** and **Metasploit** for post-exploitation activities.

---

##  Prerequisites

- **VirtualBox** with Ubuntu 22.04 LTS ISO:
  - **NAT Network 1**: `10.10.10.0/24` (connects to Public Server)
  - **NAT Network 2**: `192.168.2.0/24` (simulates AD Network)
- Ubuntu Server ISO (22.04)

---

## VirtualBox Network Configuration:

1️⃣ **Adapter 1 (Internal - 10.10.10.0/24)**:
   - Attached to: **NAT Network**
   - Name: `InternalNet` (same as public Ubuntu server)

2️⃣ **Adapter 2 (AD Network - 192.168.2.0/24)**:
   - Attached to: **NAT Network**
   - Name: `ADNet` (new NAT network created in VirtualBox)
   - CIDR: `192.168.2.0/24`
     
---

##  Install and Configure OpenSSH
Install SSH server:

```bash
sudo apt update
sudo apt install openssh-server -y
```

Enable SSH:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

Create a user with a weak password for brute-force testing:

```bash
sudo adduser pivotuser
```
Set password (e.g., 123456).


## Create find Privilege Escalation Misconfiguration

Give testuser access to sudo find without a password:

```bash
sudo visudo
```

Add the line at the end:

```bash
pivotuser ALL=(ALL) NOPASSWD: /usr/bin/find
```
Test it works:

```bash
su - testuser
sudo find . -exec /bin/sh \; -quit
```

This should drop a root shell.




