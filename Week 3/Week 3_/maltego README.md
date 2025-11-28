# Maltego CE Installation & Setup Guide

This guide provides clear, step-by-step instructions for downloading, installing, and configuring **Maltego Community Edition (CE)** for OSINT investigations.

## 1. Download Maltego

1. Visit the official website: https://www.maltego.com
2. Click **Download Maltego**.
3. Select **Maltego CE (Community Edition)** — the free version.
4. Choose your operating system: Windows, macOS, or Linux.
5. Click **Download**.

## 2. Install Maltego

### Windows
1. Locate the downloaded `.exe` file.
2. Double-click to start the installer.
3. Accept the license agreement.
4. Choose an installation directory.
5. Click **Install** → **Finish**.

### macOS
1. Open the `.dmg` file.
2. Drag **Maltego** into the **Applications** folder.
3. Eject the disk image.
4. Launch Maltego from Applications.

### Linux
1. Make the installer executable:
   ```bash
   chmod +x Maltego-*.sh
   ```
2. Run the setup:
   ```bash
   sudo ./Maltego-*.sh
   ```
3. Follow the terminal prompts.

## 3. Create Your Maltego Account

1. Launch Maltego.
2. Click **Register** or **Create an account**.
3. Provide your name, email, username, password, and select **OSINT** as your purpose.
4. Accept the Terms of Service and submit.

## 4. Email Verification

1. Check your inbox for an email from Maltego.
2. Open the email titled **"Please confirm your email"**.
3. Click the activation link.
4. Your CE license is now activated.

## 5. First-Time Application Setup

1. Return to the Maltego app.
2. Log in using your Maltego ID and password.
3. Accept the EULA.
4. Wait for initialization to finish.

## 6. Configure Transforms

1. When the **Transform Hub** appears, select **Community Transforms**.
2. Click **Next**.
3. Select all available free transforms.
4. Click **Finish**.

## 7. Create Your First Investigation Graph

1. Go to **File → New** (or press `Ctrl + N`).
2. Open the **Entity Palette**.
3. Expand **Infrastructure**.
4. Drag the **Domain** entity onto the graph.
5. Double-click it and enter the target domain.

## 8. Run Your First Transform

1. Right-click the domain entity.
2. Select **Run Transform**.
3. Choose **To DNS Name – NS**.
4. Review the discovered DNS relationships.

## 9. Save Your Investigation

1. Go to **File → Save**.
2. Name your project.
3. Choose a save location.
4. Click **Save**.

## Verification Checklist

- Maltego launches correctly  
- Successful login  
- Community transforms installed  
- Graph creation works  
- Entities load  
- Transforms run successfully  

## Troubleshooting

### Invalid Username/Password
- Use your **Maltego ID**, not your email.
- Reset password at maltego.com/forgot-password

### No Transforms Available
- Open **Transforms → Transform Hub**
- Install Community Transforms

### Crashes
- Run as Administrator (Windows)
- Reinstall if needed

### Slow Performance
- Close heavy processes
- Reduce graph size
