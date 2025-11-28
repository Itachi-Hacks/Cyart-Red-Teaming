# Social Engineering Lab - Module 5  
**Documentation / README**

## 🎯 Objective  
This module teaches the initial phases of a social engineering campaign by performing ethical reconnaissance using OSINT tools and simulating a vishing (voice phishing) attack.

---

## 🧰 Lab Setup & Prerequisites

### **Environment**
- Kali Linux (recommended) on a VM such as VirtualBox/VMware.

### **Tools Required**
1. **Social-Engineer Toolkit (SET)**
   ```bash
   sudo apt install set
   ```

2. **PhoneInfoga** (OSINT tool for phone numbers)  
   Install via Docker:
   ```bash
   docker pull sundowndev/phoneinfoga:latest
   docker run --rm -it -p 8080:8080 sundowndev/phoneinfoga serve -p 8080
   ```
   Access via browser: http://localhost:8080

   CLI scan example:
   ```bash
   docker run --rm -it sundowndev/phoneinfoga scan -n <number>
   ```

3. **Maltego CE**  
   - Download from the official website  
   - Register for a free Community Edition license

---

## 🕵️‍♂️ Activity 1: Intelligence Gathering (OSINT)

### **1. PhoneInfoga**
- Launch the server and access the web interface.
- Enter the target phone number (example: `+15551234`).
- Expected data:
  - Carrier
  - Country
  - Phone type (mobile/landline)
  - Breach records
  - Linked accounts (if available)

**CLI Example**
```bash
docker run --rm -it sundowndev/phoneinfoga scan -n <number>
```

---

### **2. Maltego CE**
#### Procedure:
1. Create a new graph
2. Add a **Phone Number** entity
3. Run transforms, such as:
   - **To Website [Search Engine]**
   - **To Email Address [From Whois]**
   - **To Person [Match Name]**

#### Goal:
Map the target’s:
- Digital footprint  
- Emails  
- Domains  
- Possible identities  

---

## 🎙️ Activity 2: Vishing Simulation

### **1. Craft a Vishing Script**
Use gathered intel (carrier, linked domains, name, etc.)

**Example Pretext:**  
Pretend to be the target's bank security team calling to verify suspicious activity.

---

### **2. Use SET for Simulation**
```bash
sudo setoolkit
```

Navigation:
1. `1) Social-Engineering Attacks`
2. `5) Mass Mailer Attack` *(for crafting written pretext)*  
   or  
   Practice verbal script independently (SET’s vishing module is outdated)

Perform a mock call with:
- A lab partner  
- Or a recorded simulation  

Focus on:
- Confidence  
- Tone  
- Pretext consistency  

---

## 📝 Task Execution & Logging

### **Intel Gathering Log**

| Target ID | Data Source  | Information Gathered | Notes |
|----------|--------------|----------------------|-------|
| TID001   | PhoneInfoga  | Phone: 555-1234, Carrier: Verizon | Number active, mobile |
| TID001   | PhoneInfoga  | Email: j.doe@example.com | Linked breach data |
| TID001   | Maltego      | Domain: example.com | Corporate domain |
| TID001   | Maltego      | Person: John Doe | Name inferred |

---

## 📞 Vishing Scenario Summary (50 words)

“Posing as a Verizon security agent, I called John Doe regarding potential SIM-swapping attempts. Using OSINT to establish credibility, I built rapport and requested identity verification using a one-time passcode. This simulated attack demonstrated how attackers bypass two-factor authentication using pretext and urgency.”

---

## ✔️ End of Module  
This README documents the steps required to perform OSINT reconnaissance and execute a controlled vishing simulation as part of a social engineering training lab.
