---
cover: ../../.gitbook/assets/bear_mudak_poster (1).png
coverY: 0
---

# Mitre

* **Tactic: TA0001 - Initial Access**
  * **Technique: T1078 - Valid Accounts**
    * **Reasons:**
      * The article mentions that "the attacker used a malicious browser extension to capture the contractor’s account. Since there was no MFA required, the attacker was able to login to the virtual desktop" (2022-11-09)
  * **Technique: T1190 - Exploit Public-Facing Application**
    * **Reasons:**
      * The article mentions that "UNC4466 gained access to an internet-exposed Windows server, running Veritas Backup Exec version 21.0 using the Metasploit module \`exploit/multi/veritas/beagent\_sha\_auth\_rce\`" (2023-04-04)
      * The article mentions that the entry point in the organization was an Exchange server that was vulnerable to Microsoft Exchange vulnerabilities. Multiple webshells have been identified on the impacted server. (2022-09-07)



* **Tactic: TA0002 - Execution**
  * **Technique: T1053.005 - Scheduled Task/Job: At (Windows)**
    * **Reasons:**
      * The article mentions that "UNC4466 added immediate tasks to the default domain policy" and "These tasks were configured to perform actions which disabled security software, downloaded the ALPHV encryptor, then execute it" (2023-04-04)
  * **Technique: T1059 - Command and Scripting Interpreter**
    * **Reasons:**
      * “Before the encryption starts, the ESXi encryptor of almost any ransomware family tries to shut down running virtual machines with either the esxcli command or vim-cmd vmsvc/power.off.” (2022-11-02)
      * “Most of the ESXi encryptors require at least one parameter – the path to the target directory, that will be encrypted.” (2022-11-02)
      * The wmic process is used to extract the UUID (2022-07-19)
      * The ransomware deletes all volume shadow copies using the vssadmin.exe utility (2022-07-19)
      * There is also a second process that is responsible for deleting all volume shadow copies with wmic (2022-07-19)
      * The binary disables Automatic Repair using the bcdedit tool (2022-07-19)
      * The malicious process obtains the ARP table using the arp command (2022-07-19)
      * The net use command is utilized to connect to the local computer using different credentials stored in the BlackCat configuration (2022-07-19)
      * The ransomware writes multiple actions to the command line output (2022-07-19)
      * Processes spawned include cmd.exe with various commands (2022-07-19)
      * The article mentions that after gaining access to the internal network, the ransomware installed the legitimate tools MobaXterm and mottynew.exe (MobaXterm terminal). (2022-09-07)
      * The article mentions that the ransomware installed the cURL tool to download additional files. Process Hacker was also installed by the malware and could be used to dump the memory of the LSASS process. In the same directory with Process Hacker, the BlackCat ransomware dropped a copy of the PEView tool, which is a viewer for Portable Executable (PE) files. (2022-09-07)
  * **Technique: T1059.001 - Command and Scripting Interpreter: PowerShell**
    * **Reasons:**
      * The article mentions that "UNC4466 also used the built in Set-MpPrefernce cmdlet to disable Microsoft Defender’s real-time monitoring capability" (2023-04-04)
  * **Technique: T1059.003 - Command and Scripting Interpreter: Windows Command Shell**
    * **Reasons:**
      * The article mentions that "The attackers used batch files to execute multiple PsExec commands to deploy payloads to the identified machines" (2022-11-09)
      * The article mentions that "Get device UUID", "Stop IIS service", "Clean shadow copies", and "List Windows event logs names and try to clear them all" (2022-11-09)
  * **Technique: T1047 - Windows Management Instrumentation**
    * **Reasons:**
      * There is also a second process that is responsible for deleting all volume shadow copies with wmic (2022-07-19)
      * Processes spawned include cmd.exe with wmic commands (2022-07-19)
  * **Technique: T1569.002 - System Services: Service Execution**
    * **Reasons:**
      * A targeted service is opened by calling the OpenServiceW routine (2022-07-19)
  * **Technique: T1057.001 Process Discovery: Windows API**
    * **Reasons:**
      * The executable takes a snapshot of all processes and threads in the system (2022-07-19)
      * The processes are enumerated using the Process32FirstW and Process32NextW APIs (2022-07-19)



* **Tactic: TA0005 - Defense Evasion**
  * **Technique: T1140 - Deobfuscate/Decode Files or Information**
    * **Reasons:**
      * The content of the ransom note and the text that will appear on the Desktop Wallpaper are decrypted by the ransomware (2022-07-19)
      * The BlackCat configuration is stored in JSON form and is decrypted at runtime (2022-07-19)
      * The buffer that contains the AES key is encrypted with the RSA public key from the BlackCat configuration (2022-07-19)
  * **Technique: T1112 - Modify Registry**
    * **Reasons:**
      * The binary disables Automatic Repair using the bcdedit tool (2022-07-19)
  * **Technique: T1112.001 Modify Registry: Windows Registry**
    * **Reasons:**
      * Processes spawned include cmd.exe with a command to modify the registry (2022-07-19)
  * **Technique: T1562.001 - Impair Defenses: Disable or Modify Tools**
    * **Reasons:**
      * The article mentions that "UNC4466 added immediate tasks to the default domain policy" and "These tasks were configured to perform actions which disabled security software, downloaded the ALPHV encryptor, then execute it" (2023-04-04)
      * The article mentions that "UNC4466 also used the built in Set-MpPrefernce cmdlet to disable Microsoft Defender’s real-time monitoring capability" (2023-04-04)
      * The ransomware deletes all volume shadow copies using the vssadmin.exe utility (2022-07-19)
      * There is also a second process that is responsible for deleting all volume shadow copies with wmic (2022-07-19)
  * **Technique: T1070.001 - Indicator Removal on Host: Clear Windows Event Logs**
    * **Reasons:**
      * The article mentions that "Apart from clearing event logs" (2023-04-04)
      * The article mentions that "List Windows event logs names and try to clear them all" (2022-11-09)
      * The ransomware tries to clear all event logs, however, the command is incorrect and returns an error (2022-07-19)
  * **Technique: T1070.004 - Indicator Removal: File Deletion**
    * **Reasons:**
      * The article mentions that “Self-destruct: Configuration option, which, when enabled, will make the tool self-destruct and quit if executed in a non-corporate environment (outside of a Windows domain).” (2022-09-25)



* **Tactic: TA0007 - Discovery**
  * **Technique: T1087.002 - Account Discovery: Domain Account**
    * **Reasons:**
      * The article mentions that "UNC4466 also made use of ADRecon to gather network, account, and host information in the victim’s environment" (2023-04-04
  * **Technique: T1016 - System Network Configuration Discovery**
    * **Reasons:**
      * The article mentions that "UNC4466 used Internet Explorer to download Famatech’s Advanced IP Scanner" and "This tool is capable of scanning individual IP addresses or IP address ranges for open ports" (2023-04-04)
  * **Technique: T1082 - System Information Discovery**
    * **Reasons:**
      * The article mentions that "Get device UUID" (2022-11-09)
      * The malicious binary obtains information about the current system via a function call to GetSystemInfo (2022-07-19)
  * **Technique: T1069.002 - Permission Groups Discovery: Domain Groups**
    * **Reasons:**
      * There is a call to SHTestTokenMembership that verifies whether the user token is a member of the Administrators group in the built-in domain (2022-07-19)
  * **Technique: T1069.001 - Permission Groups Discovery: Local Groups**
    * **Reasons:**
      * BlackCat extracts a TOKEN\_GROUPS structure containing the group accounts associated with the above token using the NtQueryInformationToken function (2022-07-19)
  * **Technique: T1007.001 System Service Discovery: Windows Service Enumeration**
    * **Reasons:**
      * The process obtains a list of active services using EnumServicesStatusExW (2022-07-19)



* **Tactic: TA0004 - Privilege Escalation**
  * **Technique: T1027 - Obfuscated Files or Information**
    * **Reasons:**
      * The article mentions that "The second one started to encrypt the configuration, where the decryption key is passed via an argument named 'access token'. In other words, the latest version of BlackCat cannot be executed or have its configuration extracted if the access token is unknown" (2022-11-09)
  * **Technique: T1078 - Valid Accounts**
    * **Reasons:**
      * The article mentions that "the attacker was able to login to the virtual desktop, escalate privileges" (2022-11-09)
  * **Technique: T1134 - Access Token Manipulation**
    * **Reasons:**
      * The process opens the access token associated with the current process (2022-07-19)
      * BlackCat extracts a TOKEN\_GROUPS structure containing the group accounts associated with the above token using the NtQueryInformationToken function (2022-07-19)
  * **Technique: T1548.002 - Abuse Elevation Control Mechanism: Bypass User Access Control**
    * **Reasons:**
      * Privilege escalation via UAC bypass using CMSTPLUA COM interface (2022-07-19)
      * BlackCat ransomware uses the auto-elevated CMSTPLUA interface {3E5FC7F9-9A51-4367-9063-A120244FBEC7} in order to escalate privileges (2022-07-19)



* **Tactic: TA0006 - Credential Access**
  * **Technique: T1555 - Credentials from Password Stores**
    * **Reasons:**
      * The BlackCat configuration contains stolen credentials specific to the victim’s environment (2022-07-19)
  * **Technique: T1003.001 - OS Credential Dumping: LSASS Memory**
    * **Reasons:**
      * The article mentions that "The threat actor utilized multiple credential access tools, including Mimikatz, LaZagne and Nanodump to gather clear-text credentials and credential material" and "Nanodump was also used to dump LSASS memory" (2023-04-04)
      * The article mentions that as we already know from the malware analysis of the ransomware, BlackCat steals credentials from the victim’s environment and incorporates them into its configuration (“credentials” field). Mimikatz was utilized to dump the credentials. (2022-09-07)
  * **Technique: T1003 - OS Credential Dumping**
    * **Reasons:**
      * The article mentions that “At least one affiliate of the Noberus ransomware operation was spotted in late August using information-stealing malware that is designed to steal credentials stored by Veeam backup software.” (2022-09-25)



* **Tactic: TA0008 - Lateral Movement**
  * **Technique: T1021.002 - Remote Services: SMB/Windows Admin Shares**
    * **Reasons:**
      * The article mentions that "they used PsExec and a compromised domain account to deploy ExMatter to more than 2,000 machines in the network" (2022-11-09)
  * **Technique: T1021.001 - Remote Services: Remote Desktop Protocol**
    * **Reasons:**
      * The article mentions that after dumping credentials using Mimikatz, the malware pivots from one machine to another via Remote Desktop Protocol (RDP). (2022-09-07)



* **Tactic: TA0009 - Collection**
  * **Technique: T1560 - Archive Collected Data**
    * **Reasons:**
      * The article mentions that “Exmatter was designed to steal specific file types from a number of selected directories and upload them to an attacker-controlled server prior to deployment of the ransomware itself on the victim’s network.” (2022-09-25)



* **Tactic: TA0010 - Exfiltration**
  * **Technique: T1041 - Exfiltration Over C2 Channel**
    * **Reasons:**
      * The article mentions that "the attackers used a .NET data exfiltration tool known as ExMatter" and "this specific sample is trying to communicate with an IP address via WebDav" (2022-11-09)
  * **Technique: T1022 - Data Encrypted**
    * **Reasons:**
      * The CreateFileW API is used to open a targeted file (2022-07-19)
      * The malware generates 16 random bytes that will be used to derive the AES key (2022-07-19)
      * A JSON form containing the encryption cipher (AES), the AES key used to encrypt the file, the data, and the chunk size, is constructed in the process memory (2022-07-19).
  * **Technique: T1567.002 - Exfiltration Over Web Service: Exfiltration to Cloud Storage**
    * **Reasons:**
      * The article mentions that once the threat actor decided which files to exfiltrate, the malware compressed them using WinRAR or 7zip. The ransomware installed a tool called rclone that is utilized to upload data to cloud storage providers. A second tool called MEGAsync is also installed by the process, which can upload data to the MEGA Cloud Storage. (2022-09-07)



* **Tactic: TA0011 - Command and Control**
  * **Technique: T1090.001 - Proxy: Internal Proxy**
    * **Reasons:**
      * The article mentions that "UNC4466 made heavy use of the Background Intelligent Transfer Service (BITS) to download additional tools such as LAZAGNE, LIGOLO, WINSW, RCLONE, and finally the ALPHV ransomware encryptor" (2023-04-04)
  * **Technique: T1105 - Ingress Tool Transfer**
    * **Reasons:**
      * The article mentions that "UNC4466 made heavy use of the Background Intelligent Transfer Service (BITS) to download additional tools such as LAZAGNE, LIGOLO, WINSW, RCLONE, and finally the ALPHV ransomware encryptor" (2023-04-04)
      * The article mentions that "UNC4466 used Internet Explorer to download Famatech’s Advanced IP Scanner" (2023-04-04)
      * The article mentions that after gaining access to the internal network, the ransomware installed the legitimate tools MobaXterm and mottynew.exe (MobaXterm terminal). (2022-09-07)
      * The article mentions that the ransomware installed the cURL tool to download additional files. Process Hacker was also installed by the malware and could be used to dump the memory of the LSASS process. In the same directory with Process Hacker, the BlackCat ransomware dropped a copy of the PEView tool, which is a viewer for Portable Executable (PE) files. (2022-09-07)



* **Tactic: TA0040 - Impact**
  * **Technique: T1490 - Inhibit System Recovery: Disable System Tools**
    * **Reasons:**
      * The article mentions that "Clean shadow copies" (2022-11-09)
      * The ransomware deletes all volume shadow copies using the vssadmin.exe utility (2022-07-19)
      * There is also a second process that is responsible for deleting all volume shadow copies with wmic (2022-07-19)
      * The binary disables Automatic Repair using the bcdedit tool (2022-07-19)
  * **Technique: T1491.001 - Defacement: Internal Defacement**
    * **Reasons:**
      * The content of the ransom note and the text that will appear on the Desktop Wallpaper are decrypted by the ransomware (2022-07-19)
  * **Technique: T1486 - Data Encrypted for Impact**
    * **Reasons:**
      * The article mentions that "UNC4466 deploys the Rust-based ALPHV ransomware" (2023-04-04)
      * The article mentions that “The ransomware also offered two encryption algorithms (ChaCha20 and AES), as well as four encryption modes - Full, Fast, DotPattern and SmartPattern.” (2022-09-25)
      * The article mentions that “Coreid claims that Noberus is capable of encrypting files on Windows, EXSI, Debian, ReadyNAS, and Synology operating systems.” (2022-09-25)
      * Interestingly, BlackCat creates intermediary files called “checkpoints-\<encrypted file name>” during the encryption process (2022-07-19)
  * **Technique: T1489 - Service Stop**
    * **Reasons:**
      * The article mentions that "Stop IIS service" (2022-11-09)
      * “Before the encryption starts, the ESXi encryptor of almost any ransomware family tries to shut down running virtual machines with either the esxcli command or vim-cmd vmsvc/power.off.” (2022-11-02)
      * BlackCat stops the targeted service using the ControlService function (2022-07-19)



* **Tactic: TA0043 - Reconnaissance**
  * **Technique: T1595 - Active Scanning**
    * **Reasons:**
      * The article mentions that "one commercial Internet scanning service reported over 8500 IP addresses which advertise the 'Symantec/Veritas Backup Exec ndmp' service on the default port 10000, as well as port 9000 and port 10001" (2023-04-04)
  * **Technique: T1592 -  Gather Victim Host Information**
    * **Reasons:**
      * The article mentions that "UNC4466 used Internet Explorer to download Famatech’s Advanced IP Scanner" and "This tool is capable of scanning individual IP addresses or IP address ranges for open ports, and returns hostnames, operating system and hardware manufacturer information" (2023-04-04)