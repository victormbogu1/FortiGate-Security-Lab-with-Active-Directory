# FortiGate-Security-Lab-with-Active-Directory
A hands-on lab simulating an enterprise network environment with FortiGate firewall. Includes Web Filtering, Application Control, and Intrusion Prevention System (IPS). Demonstrates ability to secure client traffic and monitor network activity.

## Project Overview
This project is a hands-on lab built in Hyper-V with **firewall, routing, and security concepts** using a FortiGate virtual appliance.  
It simulates a small enterprise environment with:
- **FortiGate Firewall** (LAN/WAN separation, NAT, policies)
- **Windows Server (Domain Controller + DHCP/DNS)**
- **Windows 10 Client** (test machine)

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/bad93f68b6de5140f8ef998048ed8ae4969c0dc7/Diagram.png)


## Lab Environment
- FortiGate VM
- Windows Server 2019 (AD/DHCP/DNS)
- Windows 10 Test Client VM
- Hypervisor: Hyper-V

---
## 🔧 Setup Steps
1. Imported FortiGate `.vhd` into Hyper-V  
2. Configured **two network adapters** (External = WAN, Internal = LAN)  
3. Assigned LAN IP = `192.168.1.160`, WAN = external DHCP/Static  
4. Disabled DHCP on FortiGate, kept DC as DHCP server  
5. Added firewall policy (LAN → WAN, NAT enabled)  
6. Verified Internet access from test VM through FortiGate

## 🔐 Features Tested
- ✅ Basic Routing & NAT  
- ✅ AD Integration (DHCP, DNS)  
- ✅ Firewall Policies  
- ✅ Web GUI access via `https://192.168.1.160

## Routing Interface
- **WAN (port1)** → External switch (Internet access)
- **LAN (port2)** → Internal switch (AD + clients)
- **Domain Controller** → Provides DHCP & DNS
- **Clients** → Get IP from DC, gateway = FortiGate

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/b24b48a43adcef20341323ce9d86a73a6a13862b/configure-port.png)

## LAN → WAN Firewall Policy

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/485781630222a7affd00da3a1c913dd980daa5fe/configure_firewall.png)

## Fortigate Firewall Policy Control

- **Web Filtering:** Blocking websites by category or custom URLs.
- **Application Control:** Controlling/blocking apps like P2P, Instant Messaging, and games.
- **Intrusion Prevention System (IPS):** Detecting/blocking malicious traffic.

## Step 1 – Web Filtering

**Screenshot:**
![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/facebook.png)

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Web-Fliter.png)

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Instagram.png)

The below shows the log files from Fortigate with right source interface/IP from the security events
![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Event-Security.png)

---

## Step 2 – Application Control
Configuring the application Contorl 

**Screenshot:**

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/96dcfe6635367d5ce5c059aaca0c710725297f98/Application%20Control.png)

---

## Step 3 – Intrusion Prevention System (IPS)

**Screenshot:**

![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/96dcfe6635367d5ce5c059aaca0c710725297f98/IPS.png)


---

## Step 4 – Verification
- Confirm LAN → WAN policy has Security Profiles enabled.
- Test browsing, applications, and IPS triggers from the client machine.
- Include screenshots of blocked sites, blocked apps, and IPS logs.

---

## Notes
- SSL inspection requires FortiGate CA certificate installed on client machines for HTTPS filtering.
- Deep Inspection may block legitimate traffic if clients don’t trust the certificate.


