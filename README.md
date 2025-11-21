🛡️ Incident Response Capstone Project

A Complete End-to-End Detection, Analysis, and Response Workflow

📌 Introduction

This project showcases a full Incident Response (IR) lifecycle implemented in a simulated enterprise security environment. It integrates Wazuh SIEM, Suricata IDS, and CrowdSec for automated defense, paired with Windows endpoints as victims and Kali Linux as the attacker.

Using real attack simulation, log analysis, alert triage, and response actions, this project demonstrates hands-on security monitoring, threat detection, network defense, and forensic investigation.
The repository includes documentation, detection rules, screenshots, reports, and risk matrices to reflect a professional IR workflow aligned with industry frameworks such as MITRE ATT&CK, SANS Incident Handler's Process, and SOC best practices.

This project is suitable for:
✔ SOC Analysts
✔ Blue Teamers
✔ Cybersecurity Students
✔ Anyone building a cybersecurity portfolio

📁 Repository Contents

This repository contains all artifacts and documents used during the investigation:

task-2/
│
├── ALE_Risk_Matrix_Detailed.xlsx         # Asset/Loss estimation and risk matrix
├── Incident Response Capstone Project.md # IR documentation & notes
├── SANS_Incident_Report.docx             # Formal incident report using SANS template
│
├── Screenshots/                          # Wazuh & Suricata alert captures
│   ├── Screenshot 2025-11-20 03-28-03.png
│   ├── Screenshot 2025-11-20 03-28-20.png
│   ├── Screenshot 2025-11-20 03-28-41.png
│   └── suricata.PNG
│
├── Wazuh.pdf                             # Wazuh detection logs
├── Wazuh-2.pdf
├── Wazuh-3.pdf
├── point8-1.pdf                          # Additional analysis PDFs
├── point8-2.pdf
├── point8-3.pdf
│
├── wazuh_powershell_detection_readme.md  # Custom Wazuh PowerShell detection rules
└── suricata_network_defense_readme.md    # Suricata network defense documentation


🧪 Project Overview
🔥 Attack Scenario

A Kali Linux attacker (172.20.10.4) attempted to compromise Windows endpoints using:

NTLM logon attempts (Logon Type 3)

Privilege escalation (Event ID 4672)

Valid account misuse (Event ID 4634)

Lateral movement attempts

Potential brute-force activity

Wazuh generated alerts, Suricata captured suspicious traffic, and CrowdSec automatically blocked the attacker's IP.


Architecture Diagram


┌──────────────────┐
│  Kali Attacker   │
│   172.20.10.4    │
└───────┬──────────┘
        │
 NTLM Logon Attempts
        │
┌──────────────────┴────────────────────┐
│                                       │
┌─────────────────┐                    ┌────────────────────┐
│ Windows 10 Host │                    │ Windows Server 2019│
│ WINDOWS10PROOO  │                    │ WINDOWS19PROOO     │
└─────────┬───────┘                    └─────────┬──────────┘
        │                                      │
        └──────────────┬──────────────────────┘
                       Log Forwarding to Wazuh
                              │
                       ┌──────▼────────┐
                       │   Wazuh SIEM  │
                       │ Detection & IR│
                       └──────┬────────┘
                              │
                              ▼
                      ┌───────────────┐
                      │   CrowdSec    │
                      │ Auto-Blocking │
                      └───────────────┘


🧠 Key Findings
✔ Multiple NTLM Network Logon (Type 3) attempts

Indicating a brute-force or credential-stuffing attempt.

✔ Privileged Login Detected (Event ID 4672)

Administrator account received high-level privileges such as:

SeDebugPrivilege

SeBackupPrivilege

SeRestorePrivilege

This is often associated with:

Credential theft

Privilege escalation

Persistence attempts

✔ Suspicious Logoff (Event ID 4634)

Signs of account misuse or lateral movement.

✔ CrowdSec Blocked Attacker IP

172.20.10.4 banned automatically.
Ping tests confirmed containment.

🛠 Tools & Technologies

| Component                    | Role                            |
| ---------------------------- | ------------------------------- |
| **Wazuh**                    | SIEM, Log analysis, Alerting    |
| **Suricata**                 | Network IDS, Traffic inspection |
| **CrowdSec**                 | Automated IP blocking           |
| **Windows 10 / Server 2019** | Victim endpoints                |
| **Kali Linux**               | Attacker machine                |
| **Excel**                    | Risk matrix & ALE calculation   |
| **SANS Template**            | Formal incident reporting       |

📑 Documentation Included
This repository contains professional-grade documents that illustrate the IR process:

📄 SANS Incident Report
A formal write-up of the entire incident.

📄 Wazuh Detection Logs
PDF files showing alerts triggered by the attack campaign.

📄 Risk Matrix (ALE)
A complete risk evaluation for impacted assets.

📄 Suricata Network Defense Guide
Explains IDS triggers & configuration.

📄 Wazuh PowerShell Detection Rules
Custom rules for detecting malicious script execution.

🧭 Learning Outcomes

By completing this project, the following skills were demonstrated:

✔ SIEM setup & threat detection
✔ Windows event log analysis
✔ Network intrusion detection using Suricata
✔ Automated defense integration with CrowdSec
✔ Mapping attacker behavior to MITRE ATT&CK
✔ Writing professional incident response reports (SANS Format)
✔ Executing an end-to-end Incident Response lifecycle
