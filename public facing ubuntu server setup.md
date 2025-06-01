#  Internal Ubuntu 20.04 Server Setup (Penetration Testing Lab)

This guide outlines the detailed setup steps for the **internal Ubuntu 20.04 server** in the penetration testing lab. This machine simulates a typical internal system with misconfigurations and weak security practices, making it a target for privilege escalation and lateral movement.

---



## 1️ Install Ubuntu 20.04 Server

- Download the Ubuntu 20.04 LTS ISO.
- Install on a VM or physical machine with the following configuration:
  - **eth0** (internal): `10.10.10.x/24`
  - **eth1** (AD network): `192.168.2.x/24`
- Assign static IPs during or after installation.





## 2️ Install Apache Web Server

```bash
sudo apt update
sudo apt install apache2 -y
```

  Start and enable Apache:
```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```




## 3️ Create Vulnerable Web Application

Create a basic Command Injection page:

```bash
sudo nano /var/www/html/cmdinj.php
```

Set proper permissions:

```bash
sudo chown www-data:www-data /var/www/html/cmdinj.php
sudo chmod 644 /var/www/html/cmdinj.php
```

Test:
```bash
Visit http://<Ubuntu-External-IP>/cmdinj.php?ip=127.0.0.1 in your browser.
```




## 4️ Setup Cronjob for Privilege Escalation

Create a root-owned script that gets executed via cron:

```bash
sudo nano /path/to/priv_esc.sh
```

Example script:
```bash
#!/bin/bash
chmod +s /bin/bash
```

Add a cronjob entry:

```bash
sudo crontab -e
```

Add the following line (runs every minute):

```bash
* * * * * /path/to/priv_esc.sh
```




