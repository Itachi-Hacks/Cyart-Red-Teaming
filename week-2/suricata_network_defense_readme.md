# Network Defense with Open‑Source Tools: Suricata Setup & Test Rules

This guide walks you through installing **Suricata**, creating safe test rules, running the IDS, and validating alert logs. It also includes example detections mapped to MITRE ATT&CK techniques.

---

## 🛠️ Tools Used
- **Suricata** — Open‑source IDS/IPS for network threat detection.

---

## 🚀 Installation
Run the following command to install Suricata and update rule sources:
```bash
sudo apt update && sudo apt install -y suricata suricata-update && sudo suricata-update && sudo systemctl enable --now suricata
```

This installs Suricata, enables automatic updates, and starts the service.

---

## 📁 Create Custom Rules
Navigate to the rules directory:
```bash
cd /etc/suricata/rules
```

Create a new rule file:
```bash
sudo nano local.rules
```

Paste the following safe test rule pack:

```
############################################
SURICATA TEST RULE PACK (SAFE)
############################################

1. ICMP Ping Test
alert icmp any any -> any any (msg:"TEST - ICMP ping detected"; sid:1000001; rev:1;)

2. HTTP Keyword Match
alert http any any -> any any (msg:"TEST - HTTP keyword match"; content:"test123"; http_uri; sid:1000002; rev:1;)

3. DNS Query Test
alert dns any any -> any any (msg:"TEST - DNS query for example.com"; dns.query; content:"example.com"; sid:1000003; rev:1;)

4. TLS SNI Match
alert tls any any -> any any (msg:"TEST - TLS SNI test.local"; tls.sni; content:"test.local"; sid:1000004; rev:1;)

5. TCP Port Connection Test (4444)
alert tcp any any -> any 4444 (msg:"TEST - Connection to TCP port 4444"; sid:1000005; rev:1;)

6. Base64 Pattern Match (dGVzdA==)
alert tcp any any -> any any (msg:"TEST - base64 pattern match"; content:"dGVzdA=="; sid:1000006; rev:1;)

7. HTTP User-Agent Test
alert http any any -> any any (msg:"TEST - HTTP User-Agent match"; content:"SuricataTest"; http_user_agent; sid:1000007; rev:1;)

8. Fake Malware Keyword Detection
alert tcp any any -> any any (msg:"TEST - Fake malware string detected"; content:"malware_test_string"; sid:1000008; rev:1;)

9. PowerShell Reverse Shell Keyword
alert http any any -> any any (msg:"TEST - Powershell reverse shell keyword"; content:"powershell -nop -w hidden"; sid:1000009; rev:1;)

10. Suspicious Curl Command
alert http any any -> any any (msg:"TEST - Suspicious curl UA"; content:"curltestscanner"; http_user_agent; sid:1000010; rev:1;)
```

Save and exit.

---

## ▶️ Start Suricata
Run Suricata manually using your network interface (example: **wlan0**):
```bash
sudo suricata -c /etc/suricata/suricata.yaml -i wlan0
```

Ensure your actual interface name using:
```bash
ip a
```

---

## 📄 Check Suricata Logs
View alerts:
```bash
cat /var/log/suricata/fast.log
```

(Your Suricata version may also use `eve.json`.)

---

## 📚 Reference
For more advanced detection and Wazuh integration:
- https://wazuh.com/blog/responding-to-network-attacks-with-suricata-and-wazuh-xdr/

---

## 🛡️ Example Alerts & MITRE Mappings
| Alert | Tactic | Technique | Notes |
|-------|--------|-----------|--------|
| **Possible Kali Linux hostname in DHCP Request Packet** | Discovery | **T1016 – System Network Configuration Discovery** | Indicates a system (commonly attacker‑associated) is requesting DHCP configuration details. Useful for reconnaissance. |
| **Possible Spotify P2P Client** | Command and Control | **T1573 – Encrypted Channel** | Spotify P2P is encrypted and can be abused by attackers as covert C2 traffic. Related to **T1071** (Application Layer Protocol). |

---

## ✅ Summary
This setup provides:
- A fully functional Suricata IDS installation.
- Custom safe test rules.
- Ability to monitor and validate Suricata outputs.
- Basic MITRE ATT&CK mapping for network security events.

This README is GitHub‑ready and structured for learning, labs, and documentation.

