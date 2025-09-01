# FortiGate-Security-Lab-with-Active-Directory

A hands-on lab simulating an enterprise network environment with FortiGate firewall. Includes Web Filtering, Application Control, and Intrusion Prevention System (IPS). Demonstrates ability to secure client traffic and monitor network activity.

## Project Overview

This project is a hands-on lab built in Hyper-V with firewall, routing, and security concepts using a FortiGate virtual appliance.

The lab simulates a small enterprise environment, allowing testing of security policies, monitoring, and integration with Active Directory. This reflects real-world scenarios where FortiGate is used to protect organizational networks.

- It includes:

- FortiGate Firewall – Performs LAN/WAN separation, NAT, and security enforcement.

- Windows Server (Domain Controller + DHCP/DNS) – Provides central authentication, IP addressing, and name resolution.

- Windows 10 Client – Acts as a user endpoint for testing security policies.

![Fortigate.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/bad93f68b6de5140f8ef998048ed8ae4969c0dc7/Diagram.png)


## Lab Environment

- FortiGate VM
- Windows Server 2019 (AD/DHCP/DNS)
- Windows 10 Test Client VM
- Hypervisor: Hyper-V

📌 Note: This environment was chosen because it closely resembles a typical enterprise setup where a firewall integrates with domain services for identity-based access control.

---
## 🔧 Setup Steps
1. Imported FortiGate `.vhd` into Hyper-V  
2. Configured **two network adapters** (External = WAN, Internal = LAN)  
3. Assigned LAN IP = `192.168.100.160`, WAN = external DHCP/Static  
4. Disabled DHCP on FortiGate, kept DC as DHCP server
5. Domain Machine - Static IP (192.168.100.1)
6. Client Machine - Static IP (192.168.100.6)
7. Standalone Test-PC Static IP (192.168.100.170)
8. Added firewall policy (LAN → WAN, NAT enabled)  
9. Verified Internet access from test VM through FortiGate

📌 Explanation: These steps establish the foundation of the lab. The FortiGate is placed in the traffic path, ensuring that all outbound traffic passes through it. By disabling DHCP on the FortiGate and keeping it on the Domain Controller, central control of IP management remains with Active Directory mirroring enterprise practices.

## 🔐 Features Tested
- ✅ Basic Routing & NAT  
- ✅ AD Integration (DHCP, DNS)  
- ✅ Firewall Policies  
- ✅ Web GUI access via `(192.168.1.160)

📌 Note: These features confirm that the firewall is operational, integrated into the domain network, and accessible for management.

## Routing Interface
- **WAN (port1)** → External switch (Internet access) (DHCP)
- **LAN (port2)** → Internal switch (AD + clients) (192.168.100.160)
- **Domain Controller** → Provides DHCP & DNS
- **Clients** → Get IP from DC, gateway = FortiGate (192.168.100.160)

📌 Explanation: This design places the FortiGate in line as the default gateway, forcing all LAN-to-WAN traffic through it. This allows applying security inspection and ensures visibility across the network.

![configure-port.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/b24b48a43adcef20341323ce9d86a73a6a13862b/configure-port.png)

## LAN → WAN Firewall Policy

📌 Description: The firewall policy allows internal hosts to reach the Internet while applying NAT and enabling security profiles. This step is crucial because firewall policies are the enforcement points for security features such as filtering and intrusion prevention.

![configure_firewall.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/485781630222a7affd00da3a1c913dd980daa5fe/configure_firewall.png)

## Fortigate Firewall Policy Control

- FortiGate security profiles provide layered protection:

- Web Filtering: Blocking websites by category or custom URLs.

- Application Control: Controlling/blocking apps like P2P, Instant Messaging, and games.

- Intrusion Prevention System (IPS): Detecting/blocking malicious traffic.

📌 Note: These features demonstrate FortiGate’s ability to enforce corporate policies, prevent productivity loss, and block cyber threats.

## Step 1 – Web Filtering

📌 Description: Web Filtering ensures compliance and productivity by blocking inappropriate, malicious, or non-business-related websites. In this lab, social media platforms like Facebook and Instagram were blocked to demonstrate policy enforcement.

**Screenshot:**
![facebook.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/facebook.png)

![Web-Fliter.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Web-Fliter.png)

![Instagram.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Instagram.png)

The below shows the log files from Fortigate with right source interface/IP from the security events

![Event-Security.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/d364bd8601cf1813140b97d2677f7a0bb59c6d9d/Event-Security.png)

---

## Step 2 – Application Control

📌 Description: Application Control enables identification and restriction of specific applications, even if they use non-standard ports. This is essential in enterprises where apps like torrent clients, games, or chat software may bypass normal web filters.

**Screenshot:**

![ApplicationControl.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/96dcfe6635367d5ce5c059aaca0c710725297f98/Application%20Control.png)

---

## Step 3 – Intrusion Prevention System (IPS)

📌 Description: IPS detects and blocks suspicious or malicious network traffic. In a real-world scenario, IPS protects against exploits, worms, and unauthorized scans. In this lab, it demonstrates FortiGate’s ability to detect and prevent simulated threats.

**Screenshot:**

![IPS.png.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/96dcfe6635367d5ce5c059aaca0c710725297f98/IPS.png)


---

## Step 4 – Verification

📌 Description: Verification ensures that all security profiles are active and functional. The test included domain-joined machines, a standalone PC, and a test client, confirming that traffic inspection works regardless of whether the machine is in the domain or standalone.

### Verification points:

- LAN → WAN policy had security profiles enabled.

- Browsing tests confirmed blocked websites.

- IPS detected malicious traffic attempts.

![Domain%20Machine.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/7ba358b0108f0e8cd48c843faf8f8b7d7fa0b6ff/Domain%20Machine.png)

![Clent%20Machine](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/7ba358b0108f0e8cd48c843faf8f8b7d7fa0b6ff/Clent%20Machine.png)

![Stanalone%20Machine.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/7ba358b0108f0e8cd48c843faf8f8b7d7fa0b6ff/Stanalone%20Machine.png)

![Test-PC%20.png](https://github.com/victormbogu1/FortiGate-Security-Lab-with-Active-Directory/blob/294a893f15ffb3194cf8192ba8a44b6202a3924a/Test-PC%20.png)

---

## Notes

📌 Description: Additional considerations include SSL inspection requirements, which need a FortiGate CA certificate on clients for HTTPS filtering. Without this, deep inspection can block legitimate traffic due to untrusted certificates.

## 🚀 Conclusion  

This project highlights the practical deployment of **FortiGate security features** in a controlled lab environment, simulating how enterprises secure their networks. By enabling **Web Filtering, Application Control, and Intrusion Prevention System (IPS)**, the configuration demonstrates how to effectively detect, block, and monitor potentially harmful traffic while ensuring visibility across the LAN-to-WAN perimeter.  

An important consideration is that **security in enterprise environments must balance usability with control**. Access should remain **limited and role-specific** for example, certificates and other sensitive resources must only be accessible to authorized administrators. This principle of least-privilege not only reduces risk but also aligns with best practices in enterprise security governance.  

One key lesson from this lab is the impact of **licensing constraints**. Some advanced features (such as IPS signature updates or deep SSL inspection) require a valid subscription license. This reflects a real-world scenario where organizations must plan and budget for licensing to unlock the full potential of their security infrastructure. Understanding these limitations is an essential part of professional network security management.  

This project was a valuable step in exploring the **core capabilities of FortiGate**, but it also opens the door to more advanced use cases, such as:  
- 🔐 **User Authentication with AD** – requiring domain users to authenticate before gaining internet access.  
- 🌐 **DMZ Implementation** – building a secure zone to host and publish a test web server for controlled external access.  
- 📑 **Granular Access Controls** – applying policies that separate user groups, ensuring least-privilege access.  

In summary, this lab was not only a technical exercise but also a demonstration of how **FortiGate integrates into enterprise security strategies**. It shows how even a simple setup can be extended into **real-world enterprise architectures**, where considerations such as **licensing, scalability, and compliance** play a critical role.  


