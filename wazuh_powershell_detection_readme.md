# Threat Hunting with Open-Source Tools: Detecting PowerShell Abuse Using Wazuh

This guide demonstrates how to detect PowerShell abuse techniques on Windows endpoints using **Wazuh 4.9.2** and open-source tools. It includes configuration steps for both the Windows endpoint and the Wazuh server, along with custom detection rules and a sample malicious execution.

---

## 🚀 Infrastructure Requirements

- **Wazuh 4.9.2** (Server, Indexer, Dashboard) installed on **Kali Linux**.  
- **Windows 11 endpoint** with the Wazuh agent installed and enrolled.

---

## 🖥️ Windows Endpoint Configuration

### 1. Enable PowerShell Logging
Run the following function in **PowerShell as Administrator** to enable Script Block Logging and Module Logging:

```powershell
function Enable-PSLogging {
    $scriptBlockPath = 'HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging'
    $moduleLoggingPath = 'HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging'

    if (-not (Test-Path $scriptBlockPath)) { New-Item $scriptBlockPath -Force }
    Set-ItemProperty -Path $scriptBlockPath -Name EnableScriptBlockLogging -Value 1

    if (-not (Test-Path $moduleLoggingPath)) { New-Item $moduleLoggingPath -Force }
    Set-ItemProperty -Path $moduleLoggingPath -Name EnableModuleLogging -Value 1

    $moduleNames = @('*')
    New-ItemProperty -Path $moduleLoggingPath -Name ModuleNames -PropertyType MultiString -Value $moduleNames -Force

    Write-Output "Script Block Logging and Module Logging have been enabled."
}

Enable-PSLogging
```

Expected output:
```
Script Block Logging and Module Logging have been enabled.
```

---

### 2. Enable PowerShell Log Forwarding in Wazuh Agent
Add the following block inside the `<ossec_config>` section of:

**`C:\Program Files (x86)\ossec-agent\ossec.conf`**

```xml
<localfile>
  <location>Microsoft-Windows-PowerShell/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

---

### 3. Restart the Wazuh Agent
```powershell
Restart-Service -Name wazuh
```

---

## 🔐 Wazuh Server Configuration
To detect PowerShell abuse, create custom rules on the server.

### 1. Add Custom Rules
Edit the file:  
**`/var/ossec/etc/rules/local_rules.xml`**

Add the following rules:

```xml
<group name="windows,powershell,">

  <rule id="100201" level="8">
    <if_sid>60009</if_sid>
    <field name="win.eventdata.payload" type="pcre2">(?i)CommandInvocation</field>
    <field name="win.system.message" type="pcre2">(?i)EncodedCommand|FromBase64String|EncodedArguments|-e\b|-enco\b|-en\b</field>
    <description>Encoded command executed via PowerShell.</description>
    <mitre>
      <id>T1059.001</id>
      <id>T1562.001</id>
    </mitre>
  </rule>

  <rule id="100202" level="4">
    <if_sid>60009</if_sid>
    <field name="win.system.message" type="pcre2">(?i)blocked by your antivirus software</field>
    <description>Windows Security blocked malicious command executed via PowerShell.</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>

  <rule id="100203" level="10">
    <if_sid>60009</if_sid>
    <field name="win.eventdata.payload" type="pcre2">(?i)CommandInvocation</field>
    <field name="win.system.message" type="pcre2">(?i)Add-Persistence|Find-AVSignature|Get-GPPPassword|Invoke-Mimikatz|Invoke-Shellcode|PowerUp|PowerView|Set-MasterBootRecord</field>
    <description>Risky CMDLet executed. Possible malicious activity detected.</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>

  <rule id="100204" level="8">
    <if_sid>91802</if_sid>
    <field name="win.eventdata.scriptBlockText" type="pcre2">(?i)mshta.*GetObject|mshta.*new ActiveXObject</field>
    <description>Mshta used to download a file. Possible malicious activity detected.</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>

  <rule id="100205" level="5">
    <if_sid>60009</if_sid>
    <field name="win.eventdata.contextInfo" type="pcre2">(?i)ExecutionPolicy bypass|exec bypass</field>
    <description>PowerShell execution policy set to bypass.</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>

  <rule id="100206" level="5">
    <if_sid>60009</if_sid>
    <field name="win.eventdata.contextInfo" type="pcre2">(?i)Invoke-WebRequest|IWR.*-url|IWR.*-InFile</field>
    <description>Invoke Webrequest executed, possible download cradle detected.</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>

</group>
```

---

### 2. Restart Wazuh Manager
```bash
sudo systemctl restart wazuh-manager
```

---

## 🧪 Execution of Malicious Commands (Test)
Run the following commands on the Windows endpoint to trigger detection:

### Download SharpHound (BloodHound Collector)
```powershell
curl "https://raw.githubusercontent.com/BloodHoundAD/BloodHound/refs/heads/master/Collectors/SharpHound.ps1" -o SharpHound.ps1
```

### Execute with Execution Policy Bypass
```powershell
powershell -ep bypass .\sharphound.ps1 --collectionmethod all
```

You should now see alerts generated in the **Wazuh Dashboard** under Security Events.

---

## 📌 Summary
This setup enables:
- Monitoring of PowerShell logs.
- Detection of encoded commands.
- Alerts for suspicious cmdlets and download cradles.
- Visibility into PowerShell misuse aligned with MITRE ATT&CK techniques.

---

## 📄 Author
Created for practical threat hunting and PowerShell abuse detection using open-source tools.

