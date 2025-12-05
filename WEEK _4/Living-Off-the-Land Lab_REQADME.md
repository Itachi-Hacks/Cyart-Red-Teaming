# 🔥 Reverse Shell Exploitation Walkthrough

This repository documents the steps taken to generate, deliver, and execute a PowerShell reverse shell, leading to full remote access on a Windows target. The exercise simulates a realistic post-exploitation workflow, including credential harvesting and system enumeration.

---

## 📌 Overview

A reverse shell was generated using revshells.com, executed on a Windows machine, and connected to a Netcat listener running on Kali Linux. Once the shell was established, reconnaissance commands were executed, and sensitive credentials were extracted from `credential.xml`.

This project is intended **for educational and ethical training only**.

---

## 🧠 Attack Log Table

| Attack ID | Tool Used | Action | Notes |
|-----------|------------|---------|--------|
| 01 | revshells.com | Generated PowerShell reverse shell | Configured for attacker IP and port. |
| 02 | nc (Netcat) | Listener started | Awaited incoming connection (port 4444). |
| 03 | PowerShell | Executed payload | Triggered outbound reverse shell. |
| 04 | Netcat Shell | Shell obtained | Verified with commands (`pwd`, `whoami`). |
| 05 | CMD/PS | Directory enumeration | Confirmed payload delivery. |
| 06 | PowerShell | Navigated filesystem | Located sensitive files. |
| 07 | PowerShell | Extracted credentials | Loaded `credential.xml`. |
| 08 | Netcat Shell | Maintained remote access | Demonstrated post-exploitation control. |

---

## 🔍 Credential Extraction Evidence

The XML PSCredential file (`credential.xml`) was successfully retrieved and parsed during the session.

```
<credential.xml content redacted for security>
```

---

## 📜 50-Word Summary

A PowerShell reverse shell was generated and executed on the target, connecting back to a Netcat listener. The attacker gained remote command execution, navigated directories, and extracted stored credentials. This demonstrates a full exploitation chain: payload delivery, shell execution, privilege discovery, and credential harvesting for potential lateral movement.

---

## 📸 Screenshots

Add the images:

```
/screenshots/task7-1.png
/screenshots/task7-4.png
/screenshots/task7-5.png
/screenshots/task7-6.png
/screenshots/task7-7.png
```

---

## ⚠ Legal Disclaimer

This repository is for **cybersecurity education and authorized testing only**.  
Unauthorized access or misuse is illegal.  
The author assumes no responsibility for unethical actions.

---
