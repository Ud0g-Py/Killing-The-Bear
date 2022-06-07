---
cover: ../../.gitbook/assets/cat2.png
coverY: 0
---

# TTPs

## TTPs

* T1003.004 - OS Credential Dumping: LSA Secrets
* T1005 - Data from Local System
* T1007 - System Service Discovery&#x20;
* T1021.001 - Remote Desktop Protocol (RDP)
* T1021.002 - SMB/Windows Admin Shares&#x20;
* T1027 - Obfuscated Files or Information&#x20;
* T1027.002 - Obfuscated Files or Information: Software Packing&#x20;
* T1047 - Windows Management Instrumentation
* T1049 - System Network Connections Discovery
* T1053.003 - Command and Scripting Interpreter: Windows Command Shell
* T1057 - Process Discovery&#x20;
* T1059 - Command and Scripting Interpreter&#x20;
* T1078 - Valid Accounts&#x20;
* T1082 - System Information Discovery&#x20;
* T1083 - File and Directory Discovery&#x20;
* T1090.003 - Multi-hop Proxy
* T1105 - Ingress Tool Transfer
* T1112 - Modify Registry
* T1129 - Shared Modules
* T1140 - Deobfuscate/Decode Files or Information&#x20;
* T1202 - Indirect Command Execution&#x20;
* T1218.003 - Signed Binary Proxy Execution: CMSTP
* T1485 - Data Destruction&#x20;
* T1486 - Data Encrypted for Impact&#x20;
* T1489 - Service Stop&#x20;
* T1490 - Inhibit System Recovery
* T1498 - Network Denial of Service
* T1499 - Endpoint Denial of Service&#x20;
* T1531 - Account Access Removal&#x20;
* T1543.003 - Create or Modify System Process: Windows Service&#x20;
* T1550.002 - Use Alternate Authentication Material: Pass the Hash&#x20;
* T1552 - Unsecured Credentials&#x20;
* T1557.001 - Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay
* T1560 - Archive Collected Data
* T1562.001 - Impar Defenses: Disable or Modify Tools
* T1567.002 - Exfiltration to Cloud Storage&#x20;
* T1569.002 - System Service: Service execution
* T1570 - Lateral Tool Transfer&#x20;

## Hunt Activity

* Suspicious SMB traffic
* 'vssadmin' shadow copy deletions
* Recovery mode edits or disables using 'bcedit.exe'
* Propagation via 'psexec'
* Use of anti-forensics tools like fileshredder
* Collection machine UUID via WMIC commands
* The universally unique identifier (UUID) is later used, together with the token, to identify the victim in a Tor website hosted by the malicious actors.
* Delete volume shadow copies.
* Increase the number of network requests that the server service can perform.
* Stop the IIS service using the iisreset.exe, a well-known tool used to handle IIS services.
* Execute arp command to display current ARP (Address Resolution Protocol) entries.
* Execute Fsutil to allow the use of both remote and local symlinks.
* Clear all event logs via wevutil.exe.

## Behaviour

### Windows

```
cmd /c wmic csproduct get UUID
vssadmin.exe delete shadows /all /quiet
powershell.exe -nop -exec bypass -EncodedCommand LgBcAHMAcAByAGUAYQBkAC4AYgBhAHQAIABtAGsAcwBoAGEAcgBlACAAUgBFAEEARAA=
icacls c:\windows\debug\app /grant "Authenticated Users":(OI)(CI)F /T
net share app)c:\windows\debug\app /grant:everyone,READ
fsutil behavior set SymlinkEvaluation R2L:1 
fsutil behavior set SymlinkEvaluation R2R:1
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters /v MaxMpxCt /d 65535 /t REG_DWORD /f
psexec.exe -accepteula \\<TARGET_HOST> -u <USERNAME> -p <PASSWORD> -s -d -f -c <ALPHV_EXECUTABLE> [FLAGS] [OPTIONS] --access-token <ACCESS_TOKEN> [SUBCOMMAND]
arp -a
%SYSTEM32%\DllHost.exe /Processid:{3E5FC7F9-9A51-4367-9063-A120244FBEC7}
for /F \"tokens=*\" %1 in ('wevtutil.exe el') DO wevtutil.exe cl \"%1\""
/c \\DOMAIN.LOCAL \netlogon\locker.exe --access-token CODE
gpupdate /force
```

### Linux / VMware ESXi

```
esxcli --formatter=csv --format-param=fields=="WorldID,DisplayName" vm process list
awk -F "\"*,\"*" '{system("esxcli vm process kill --type=force --world-id="$1)}'
for i in `vim-cmd vmsvc/getallvms| awk '{print$1}'`;do vim-cmd vmsvc/snapshot.removeall $i & done 
```
