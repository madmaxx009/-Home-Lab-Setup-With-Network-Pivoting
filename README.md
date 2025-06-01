# Home Lab Setup With Network Pivoting


##Description

This project demonstrates a **realistic multi-stage penetration testing scenario** in a custom **home lab environment**. The lab simulates an enterprise network with multiple machines, vulnerabilities, and pivot points to practice offensive security techniques.

##  Lab Architecture

| Machine | OS            | IP Address(es)                | Vulnerability                  | Goal                       |
| ------- | ------------- | ---------------------------- | ----------------------------- | -------------------------- |
| Web Server | Ubuntu         | 192.168.1.0/24, 10.10.10.0/24   | Command Injection in Apache    | Initial Access, PrivEsc     |
| Internal Server | Ubuntu         | 10.10.10.0/24, 192.168.2.0/24   | Weak SSH Password, `find` misconfig | Pivot, PrivEsc, Enumeration |
| Windows Server | Windows Server 2012 | 192.168.2.0/24                     | Rejetto HFS v2.3c RCE         | Administrator Shell         |
| Windows 7 | Windows 7       | 192.168.2.0/24                     | MS17-010 (EternalBlue)        | Administrator Shell               |

---

##  Attack Flow

### 1️ Initial Foothold: Web Server (Public-Facing Ubuntu)
- Exploited **command injection** vulnerability in a web application hosted on Apache.
- Gained a low-privilege shell.
- Escalated privileges via a **cronjob misconfiguration**.

### 2️ Pivoting to Internal Network
- Used **Chisel** for port forwarding and **proxychains** to access the internal network (`10.10.10.0/24`).

### 3️ Internal Server Exploitation (Ubuntu)
- Brute-forced **SSH credentials** using **Hydra**.
- Escalated privileges via a misconfigured **find command**.
- Installed **Nmap** and **Metasploit** for further enumeration and exploitation.
- Enumerated the **Active Directory network** (`192.168.2.0/24`).

### 4️ Windows Exploitation (AD Network)
- **Windows Server 2012**: Exploited **Rejetto HFS v2.3c** (RCE) using Metasploit to gain an **Administrator shell**.
- **Windows 7**: Exploited **MS17-010 (EternalBlue)** vulnerability for **SYSTEM-level access**.

---

##  Tools & Techniques Used
- **Chisel** (port forwarding & tunneling)
- **Proxychains** (network pivoting)
- **Hydra** (SSH brute-force)
- **Nmap** (network scanning)
- **Metasploit Framework** (exploitation)
- Privilege Escalation:  
  - **Cronjob misconfiguration**  
  - **find command misconfiguration**

---

##  Skills Demonstrated
- Multi-Stage Penetration Testing
- Network Pivoting and Tunneling
- Privilege Escalation Techniques
- Brute-Force and Exploitation
- Post-Exploitation and Lateral Movement
- Active Directory Enumeration

---

##  Network Diagram



