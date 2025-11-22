# 🛡️ Incident Response Capstone Project

## ⚙️ Tools Used
- **Wazuh** – Detection, monitoring, and alert correlation
- **Windows 10 & Windows Server 2019** – Victim endpoints
- **Kali Linux** – Attacker machine
- **CrowdSec** – Network-based defense & IP blocking

---

# 📌 Project Overview
This project demonstrates a complete **end-to-end Incident Response (IR) lifecycle**, including detection, triage, response, forensics, and recommendations. Using a simulated enterprise network, an attacker attempts credential-based compromise and lateral movement. Wazuh detects anomalies, and CrowdSec automatically isolates the threat.

---

# 🖼️ Architecture Diagram
Below is a high-level representation of the lab environment used during the investigation:

```
                  ┌────────────────┐
                  │   Kali Linux   │
                  │  (Attacker)    │
                  │ 172.20.10.4    │
                  └───────┬────────┘
                          │
          Malicious NTLM  │  Logon Attempts
                          │
        ┌──────────────────┴──────────────────┐
        │                                     │
┌───────────────┐                   ┌─────────────────┐
│ Windows 10 PRO │    Lateral       │ Windows Server   │
│  Victim Host   │ <──────────────▶ │    2019 PRO      │
│ WINDOWS10PROOO │                   │ WINDOWS19PROOO  │
└───────────────┘                   └─────────────────┘
                          │
                 Log Forwarding (Wazuh)
                          │
                 ┌────────────────────┐
                 │     Wazuh SIEM     │
                 │ Detection & Alerts │
                 └─────────┬──────────┘
                           │
                           ▼
                  ┌───────────────────┐
                  │     CrowdSec      │
                  │ Auto-block Attacks│
                  └───────────────────┘
```

---

# 🚨 Incident Summary

## 📅 Timeline of Alerts (Inferred: 2025-01-01)
| Timestamp | Source IP | Description | MITRE Technique |
|----------|------------|-------------|------------------|
| 2025-01-01 | 172.20.10.4 | NTLM Network Logon Type 3 to WINDOWS10/19 | **T1110 – Brute Force** |
| 2025-01-01 | 172.20.10.4 | Event ID 4672 – Special Privileges Assigned | **T1078 – Valid Accounts** |
| 2025-01-01 | 172.20.10.4 | Event ID 4634 – Account Logoff (admin) | **T1078 – Valid Accounts** |

---

# 🔍 Detailed Incident Analysis

### **1. Network Logon Using NTLM (Type 3)**
- The attacker initiated remote login attempts to both Windows hosts.
- NTLM authentication was used — a weaker protocol commonly exploited.
- Detection triggered in **Wazuh**.

**Image Placeholder:**  
`![Network Logon Alert](images/network_logon.png)`

---

### **2. Privilege Escalation (Event ID 4672)**
A successful authentication granted the attacker powerful privileges:
- `SeDebugPrivilege`
- `SeBackupPrivilege`
- `SeRestorePrivilege`

These privileges enable:
- Credential extraction
- Token manipulation
- Full system control

**Image Placeholder:**  
`![Event ID 4672](images/privilege_escalation.png)`

---

### **3. Suspicious Logoff (Event ID 4634)**
The attacker logs off after privilege assignment—commonly seen when:
- Harvesting credentials
- Establishing persistence
- Preparing for lateral movement

**Image Placeholder:**  
`![Event ID 4634](images/logoff_event.png)`

---

# 🛑 Containment
CrowdSec was configured to automatically ban malicious IPs based on Wazuh alerts.
- IP **172.20.10.4** was blocked.
- A post-block **ping test confirmed** the system is unreachable.

**Image Placeholder:**  
`![CrowdSec Block](images/crowdsec_block.png)`

---

# 🛠️ Actions Taken
- Blocked attacker IP using CrowdSec
- Validated the ban using network connectivity tests
- Archived Wazuh alerts and Windows logs
- Began reviewing Admin and Administrator account integrity

---

# ✔️ Recommendations

### 🔐 Strengthen Authentication
- Enforce **MFA** for all users
- Require **strong, unique passwords**
- Disable NTLM where possible → Prefer **Kerberos**

### 🛡️ Account Hardening
- Review and rotate passwords for: *Administrator*, *admin*
- Audit privileged groups (Domain Admins, Local Admins)

### 🧹 System Hardening
- Patch all Windows systems
- Restrict remote logon rights
- Enable Windows Credential Guard

### 🔍 Monitoring Improvements
Add alerts for:
- Event ID **4672** (High-privilege logons)
- Multiple failed NTLM login attempts
- Remote logon Type 3 anomalies

---

# 📁 Repository Structure
```
/Incident-Response-Project
│
├── images/                 # Screenshots for documentation
├── logs/                   # Wazuh report & Windows event logs
├── configs/                # Wazuh/CrowdSec config files
└── README.md               # Main documentation
```

---

# 🧭 Learning Outcomes
- Performed real-world incident response with live attacker simulation
- Mapped adversary activity to **MITRE ATT&CK**
- Used **Wazuh** for SIEM-level visibility
- Applied **CrowdSec** for automated threat mitigation
- Gained hands-on experience detecting lateral movement & privilege misuse

---

# 📬 Contact
If you would like a downloadable **PDF version** or **graphics for the diagrams**, let me know!

