---
cover: ../../.gitbook/assets/bear_Kashkan_cover.png
coverY: 0
---

# Mitre

* Tactic: TA0001 - Initial Access
  * Technique: T1091 - Replication Through Removable Media
    * Reasons:
      * The article mentions that USB spreading malware continues to be a useful vector to gain initial access into organizations.(2023-01-06)
      * A sample of Andromeda was found on an infected USB drive in December 2021. A malicious LNK file on the drive was used for malware execution. (2023-01-06)
  * Technique: T1204.002 - User Execution: Malicious File
    * Reasons:
      * The article mentions that a malicious link file (LNK) disguised as a folder within a USB drive was clicked, leading to the automatic installation of the ANDROMEDA malware. (2023-01-06)
  * Technique: T1566.001 - Phishing: Spearphishing Attachment
    * Reasons:
      * The article mentions that UNC4210 used KOPILUWAK as a “first-stage” profiling utility, which was delivered to victims as a first-stage malicious email attachment. (2023-01-06)



* Tactic: TA0003 - Persistence
  * Technique: T1091 - Replication Through Removable Media
    * Reasons:
      * A sample of Andromeda was found on an infected USB drive in December 2021. A malicious LNK file on the drive was used for malware execution. (2023-01-06)
  * Technique: T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder
    * Reasons:
      * Immediately after infection, the Andromeda sample established persistence by adding a registry key to be executed each time the user logged in. (2023-01-06)
      * The article mentions that the ANDROMEDA malware established persistence by adding a Run Registry Key to execute it every time the system user logged on. (2023-01-06)



* Tactic: TA0005 - Defense Evasion
  * Technique: T1027 - Obfuscated Files or Information
    * Reasons:
      * Turla’s malware continuously tweaks its code over the years, indicating persistence in evading detection and maintaining access to compromised systems (2023-05-20)
      * The same self-extracting archive containing the malware was executed several times on the target system between September 6th and 8th. (2023-01-06)
      * The article mentions that the communications between QUIETCANARY and the C2 are RC4 encrypted and Base64 encoded over HTTPS. This aligns with the technique of using obfuscated files or information for defense evasion. (2023-01-06)
  * Technique: T1055 - Process Injection
    * Reasons:
      * The article describes the ANDROMEDA injected process “wuauclt.exe” making a GET request and downloading/executing a WinRAR SFX containing KOPILUWAK. This aligns with the technique of process injection. (2023-01-06)



* Tactic: TA0007 - Discovery
  * Technique: T1046 - Network Service Scanning
    * Reasons:
      * The Kopiluwak JavaScript-based reconnaissance utility was deployed on the victim’s system on September 6th. (2023-01-06)
  * Technique: T1016 - System Network Configuration Discovery
    * Reasons:
      * The article mentions that UNC4210 conducted basic network reconnaissance on the victim machine with commands like whoami, netstat, arp, and net, and piped the command results into %TEMP%\result2.dat before uploading them to the C2. (2023-01-06)



* Tactic: TA0009 - Collection
  * Technique: T1119 - Automated Collection
    * Reasons:
      * Turla’s malware, such as Snake, is designed to collect and exfiltrate data from compromised systems (2023-05-20)
  * Technique: T1115 - Clipboard Data
    * Reasons:
      * Turla’s malware may utilize clipboard data as a means to collect sensitive information from compromised systems (2023-05-20)



* Tactic: TA0010 - Exfiltration
  * Technique: T1041 - Exfiltration Over Command and Control Channel
    * Reasons:
      * The article mentions interactive commands sent to and executed by QUIETCANARY, which suggests manual exfiltration over the command and control channel. (2023-01-06)



* Tactic: TA0011 - Command and Control
  * Technique : T1105 - Ingress Tool Transfer
    * Reasons:
      * In January 2022, an old expired Andromeda C\&C domain was reregistered. UNC4210 used the domain to profile victims and then delivered the Kopiluwak dropper to those deemed interesting. (2023-01-06)
      * On September 8th, the threat actor deployed the Quietcanary .NET backdoor which is used for data harvesting and exfiltration. (2023-01-06)
      * The article describes the ANDROMEDA injected process “wuauclt.exe” making a GET request and downloading/executing a WinRAR SFX containing KOPILUWAK. (2023-01-06)
  * Technique: T1094 - Custom Command and Control Protocol
    * Reasons:
      * The article describes the C2 communication between QUIETCANARY and the hard-coded C2, which involves RC4 encryption and Base64 encoding over HTTPS. This aligns with the technique of using a custom command and control protocol. (2023-01-06)
  * Technique: T1090.003 – Proxy: Multi-hop Proxy
    * Reasons:
      * Turla’s hackers have been known to hijack satellite internet connections and use them as a proxy for their command-and-control infrastructure (2023-05-20)
