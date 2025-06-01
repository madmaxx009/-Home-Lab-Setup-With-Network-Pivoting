# Ubuntu 14.04 Server Setup for Penetration Testing Lab

This guide explains how to set up the Ubuntu 14.04 machines used in the multi-tier penetration testing lab, including configurations for networking, vulnerable services, and tools installation.

---

## 1. Install Ubuntu 14.04 Server

- Download Ubuntu 14.04 ISO from the official archive or trusted mirror.
- Install Ubuntu Server on your VM or physical machine.
- Configure networking during installation:
  - Assign static IP addresses for each network interface according to your lab design.
    - Example for public-facing server:
      - `eth0` (external): `192.168.1.X/24`
      - `eth1` (internal): `10.10.10.X/24`

---

## 2. Configure Network Interfaces

Edit `/etc/network/interfaces` to set static IPs for both NICs:

```bash
sudo nano /etc/network/interfaces
```
auto eth0
iface eth0 inet static
    address 192.168.1.X
    netmask 255.255.255.0
    gateway 192.168.1.1

auto eth1
iface eth1 inet static
    address 10.10.10.X
    netmask 255.255.255.0


Restart networking:

```bash
sudo service networking restart
```

## 3. Install and Configure Apache Web Server
```bash
sudo apt-get update
sudo apt-get install apache2 -y
```
*Place your vulnerable web application or script in /var/www/html/
*Ensure Apache is running:
```bash 
sudo service apache2 start
```
## 4. Setup Vulnerable Webpage for Command Injection

Create a PHP or CGI script vulnerable to command injection, e.g., /var/www/html/vuln.php

Test by visiting: http://192.168.1.X/vuln.php?cmd=whoami



5. Create Vulnerable Cronjob for Privilege Escalation
Edit root’s crontab:

```bash
sudo crontab -e
```

Add a cronjob running a script with insecure permissions, e.g.:
```bash
* * * * * /usr/local/bin/backup.sh
```

Create /usr/local/bin/backup.sh with insecure code allowing privilege escalation:
```bash
#!/bin/bash
cp /etc/passwd /tmp/passwd_backup
```

Set permissions so you can modify this script (e.g., world writable) to exploit.

## 6. Configure SSH with Weak Passwords (Internal Ubuntu)
Install SSH server:
```bash
sudo apt-get install openssh-server -y
Create users with weak passwords:
```

```bash
sudo adduser testuser
# Set password like "password123"
```

Ensure SSH service is running:

```bash
sudo service ssh restart
```

