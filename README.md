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

The below shows the log files from Fortigate with right source interface/IP
![Backup_Process.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Event-Security.png)

---

## Step 2 – Application Control
Configuring the application Contorl 

**Screenshot:**

---

## Step 3 – Intrusion Prevention System (IPS)
1. Go to `Security Profiles → IPS → Create New`.
2. Name the sensor `LAN-IPS`.
3. Enable critical vulnerabilities:
   - Malware
   - Anomaly
   - Optional: Botnet C&C (if available)
4. Apply to LAN → WAN firewall policy under `Security Profiles → IPS`.
5. Test using safe tools:
   - [EICAR Test File](https://www.eicar.org/download-anti-malware-testfile/)
   - Visit [testmyids.com](http://testmyids.com)
6. Enable packet logging and status for visibility.

**Screenshot Examples:**
- `IPS/LAN-IPS_Profile.png`
- `IPS/IPS_Logs_Screenshot.png`

---

## Step 4 – Verification
- Confirm LAN → WAN policy has Security Profiles enabled.
- Test browsing, applications, and IPS triggers from the client machine.
- Include screenshots of blocked sites, blocked apps, and IPS logs.

---

## Notes
- SSL inspection requires FortiGate CA certificate installed on client machines for HTTPS filtering.
- Deep Inspection may block legitimate traffic if clients don’t trust the certificate.

Configurations/fortigate_cli_config.txt Example
text
Copy code
# Interfaces
config system interface
    edit "port1"
        set ip 192.168.7.187/24
        set allowaccess ping https ssh
        set type physical
    next
    edit "port2"
        set ip 192.168.1.160/24
        set allowaccess ping https ssh
        set type physical
    next
end

# LAN → WAN Firewall Policy
config firewall policy
    edit 1
        set name "LAN-to-WAN"
        set srcintf "port2"
        set dstintf "port1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
        set utm-status enable
        set webfilter-profile "LAN-WebFilter"
        set application-list "LAN-AppControl"
        set ips-sensor "LAN-IPS"
        set ssl-ssh-profile "certificate-inspection"
    next
end
