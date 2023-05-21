---
cover: ../../.gitbook/assets/git_cover.png
coverY: 0
---

# Mitre

* **Tactic: TA0001 - Initial Access**
  * **Technique: T1078.001 - Valid Accounts: Default Accounts**
    * **Reasons:**
      * “Weak SNMP community strings, including the default “public,” allowed APT28 to gain access to router information.” (2023-04-19)
  * **Technique: T1190 - Exploit Public-Facing Application**
    * **Reasons:**
      * "APT28 has been known to access vulnerable routers by using default and weak SNMP community strings, and by exploiting CVE-2017-6742(Cisco Bug ID: CSCve54313) as published by Cisco." (2023-04-19)
  * **Technique: T1078.001 - Valid Accounts: Default Accounts**
    * **Reasons:**
      * The attackers accessed victim routers using default accounts. In particular, default/well-known SNMP community strings such as "public" were exploited, as outlined in CVE-2017-6742 (Cisco Bug ID: CSCve54313): (2023-04-19)
  * **Technique: T1566 - Phishing**
    * **Reasons:**
      * The article mentions that the Government Computer Emergency Response Team of Ukraine (CERT-UA) has observed cases of email distribution among Ukrainian government agencies with the subject "Windows Update" during April 2023. These emails are purportedly sent on behalf of department system administrators. The sender email addresses, created on the public service "@outlook.com," may be formed using the real surname and initials of an employee. (2023-04-30)
  * **Technique: T1566.001 - Phishing: Spearphishing Attachment**
    * **Reasons:**
      * The article mentions that the retrieved decoy document trick the victim into click on a button to decrypt the page content, which is presumably secured by a UKR.NET technology. (2023-05-17)
  * **Technique: T0000 - Phishing: Man in the Browser**
    * **Reasons:**
      * The article mentions that clicking on the button displays fake login window. This window contains an iframe that embeds a fake UKR.NET login webpage hosted on robot-876.frge\[.]io. The intention here is to trick the user into entering their credentials. (2023-05-17)

* **Tactic: TA0002 - Execution**
  * **Technique: T1210 - Exploitation of Remote Services**
    * **Reasons:**
      * Exploit SNMP (Simple Network Management Protocol) to gain access to the target system (2023-04-19)
  * **Technique: T1059.001 - Command and Scripting Interpreter: PowerShell**
    * **Reasons:**
      * The article mentions that a typical email contains instructions in Ukrainian regarding "updates for protection against hacker attacks" and graphical representations of the command line execution and PowerShell command. The mentioned group will download a PowerShell script that, simulating the operating system update process, ensures the download and execution of the following PowerShell script designed to gather basic computer information using "tasklist" and "systeminfo" commands and send the obtained results via an HTTP request to the Mocky service API. (2023-04-30)

* **Tactic: TA0004 - Privilege Escalation**
  * **Technique: T1068 - Exploitation for Privilege Escalation**
    * **Reasons:**
      * The article mentions that APT28 used infrastructure to masquerade SNMP access into Cisco routers worldwide. This was possible because the SNMP subsystem of Cisco IOS and IOS XE Software contains multiple vulnerabilities that could allow an authenticated, remote attacker to remotely execute code on an affected system or cause an affected system to reload. (2023-04-21)

* **Tactic: TA0005 - Defense Evasion**
  * **Technique: T1036 - Masquerading**
    * **Reasons:**
      * The article mentions that clicking on the button displays fake login window. This window contains an iframe that embeds a fake UKR.NET login webpage hosted on robot-876.frge\[.]io. The intention here is to trick the user into entering their credentials. (2023-05-17)

* **Tactic: TA0007 - Discovery**
  * **Technique: T1016.001 - System Network Configuration Discovery**
    * **Reasons:**
      * “It includes discovery of other devices on the network by querying the Address Resolution Protocol (ARP) table to obtain MAC addresses.” (2023-04-19)
  * **Technique: T1082 - System Information Discovery**
    * **Reasons:**
      * The article mentions that the mentioned group will download a PowerShell script designed to gather basic computer information using "tasklist" and "systeminfo" commands and send the obtained results via an HTTP request to the Mocky service API. (2023-04-30)

* **Tactic: TA0011 - Command and Control**
  * **Technique: T1105 - Ingress Tool Transfer**
    * **Reasons:**
      * Deploy malware to extract information from the target system. (2023-04-19)
  * **Technique: T1071.002 - Application Layer Protocol: File Transfer Protocols**
    * **Reasons:**
      * The article mentions that the actor obtained device information by executing a number of commands via the malware and send them out over trivial file transfer protocol (TFTP). The information includes discovery of other devices on the network. (2023-04-21)
      * Use TFTP (Trivial File Transfer Protocol) for communication and data exfiltration. (2023-04-19)
  * **Technique: T1071.001 - Application Layer Protocol: Web Protocols**
    * **Reasons:**
      * The article mentions that a second technique that we observed used by APT28 since more than a year is the use of public HTTP debugging / webhook services in their phishing webpages to retrieve stolen credentials. (2023-05-17)
      * The article mentions that this second-stage payload abuses the ‘tasklist’ and ‘systeminfo’ commands to gather system information and send them to a Mocky service API via an HTTP request. (2023-05-01)

* **Tactic: TA0043 - Reconnaissance**
  * **Technique: T1590 - Gather Victim Network Information**
    * **Reasons:**
      * Access was gained to perform reconnaissance on victim devices to gather information about the victim network. (2023-04-19)