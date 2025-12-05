# ⚠️ Phishing-to-Exploitation Simulation  
A cybersecurity awareness project demonstrating how a phishing webpage can socially engineer a user, deliver a payload, and simulate a ransomware-style environment.

---

## 📌 Project Overview
This repository contains a phishing simulation webpage and a demo payload.  
The webpage mimics a high-severity ransomware alert, displays fake compromise indicators, and automatically downloads a `.bat` file to replicate a real-world attack chain.

This project is for **training and educational purposes ONLY**.

---

## 📂 Files Included
- `Phishing Web.html` – High-fidelity ransomware-style phishing simulation  
- `test.bat` – Payload delivered for execution demo  
- `/screenshots/` – Images of the phishing environment and CMD outputs  
- This README.md  

---

## 🧠 Attack Chain (MITRE-Aligned)

| Phase | TTP | Tool Used | Notes |
|-------|------|-----------|-------|
| Recon | Social engineering lure | HTML phishing page | Built to trigger urgency and panic. |
| Initial Access | Phishing | Custom ransomware-themed webpage | Uses countdowns, fake logs, alerts. |
| Delivery | Forced download | JavaScript auto-download | Auto-downloads `test.bat`. |
| Execution | User-level execution | Batch script | Runs only if user manually executes it. |
| Post-Execution | Fake impact simulation | JS-rendered logs & UI | No real encryption—purely educational. |
| Impact | Data encryption / exfiltration warning | Visual simulation | Shows fake C2 IP, file encryption, etc. |

---

## 📸 Screenshots  
Add your images here:  
```
/screenshots/sim1.png
/screenshots/sim2.png
/screenshots/sim3.png
```

---

## 📜 50-Word Summary
This simulation demonstrates a realistic phishing-to-execution workflow. The webpage lures victims using a ransomware-themed interface, auto-delivers a payload, and encourages execution. It replicates attacker behavior across delivery, social engineering, and simulated impact phases, illustrating how easily human factors can lead to successful compromise.

---

## ⚠ Legal Disclaimer
This project is strictly for **cybersecurity education, awareness, and demonstration**.  
Do **NOT** deploy, share, or use these materials against unauthorized systems, individuals, or networks.  
The author is not responsible for misuse.

---
