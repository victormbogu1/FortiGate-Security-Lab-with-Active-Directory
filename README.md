# FortiGate-Security-Lab-with-Active-Directory
A hands-on lab simulating an enterprise network environment with FortiGate firewall. Includes Web Filtering, Application Control, and Intrusion Prevention System (IPS). Demonstrates ability to secure client traffic and monitor network activity.

## Project Overview
This lab simulates an enterprise network environment using a FortiGate firewall. Focused on:

- **Web Filtering:** Blocking websites by category or custom URLs.
- **Application Control:** Controlling/blocking apps like P2P, Instant Messaging, and games.
- **Intrusion Prevention System (IPS):** Detecting/blocking malicious traffic.

## Lab Environment
- FortiGate VM
- Windows Server 2019 (AD/DHCP/DNS)
- Windows 10 Test Client VM
- Hypervisor: Hyper-V

---

## Step 1 – Web Filtering
1. Go to `Security Profiles → Web Filter → Create New`.
2. Name the profile `LAN-WebFilter`.
3. Enable categories to block:
   - Social Media
   - Streaming Media
   - Adult Content
4. Add custom URLs as needed.
5. Apply profile to LAN → WAN firewall policy under `Security Profiles → Web Filter`.
6. Test from client machine:
   - Blocked sites: `facebook.com`, `instagram.com`.

**Screenshot Examples:**
- `WebFiltering/LAN-WebFilter_Profile.png`
- `WebFiltering/SocialMediaBlocked.png`

---

## Step 2 – Application Control
1. Go to `Security Profiles → Application Control → Create New`.
2. Name the profile `LAN-AppControl`.
3. Enable categories to control/block:
   - P2P/File Sharing
   - Instant Messaging (WhatsApp Web, Telegram Web)
   - Online Games
4. Apply to LAN → WAN firewall policy under `Security Profiles → Application Control`.
5. Test from client machine:
   - WhatsApp Web, Telegram Web, or Steam should be blocked.

**Screenshot Examples:**
- `ApplicationControl/LAN-AppControl_Profile.png`
- `ApplicationControl/AppBlockedExamples.png`

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
