---
cover: ../../.gitbook/assets/git_cover.png
coverY: 0
---

# Mitre

* Tactic: TA0001 - Initial Access
  * Technique: T1078 - Valid Accounts
    * Subtechnique: T1078.001 - Valid Accounts: Default Accounts
    * Reasons:
      * The attackers accessed victim routers using default accounts. In particular, default/well-known SNMP community strings such as "public" were exploited, as outlined in CVE-2017-6742 (Cisco Bug ID: CSCve54313): (2023-04)



* Tactic: TA0043 - Reconnaissance
  * Technique: T1590 - Gather Victim Network Information
    * Reasons:
      * Access was gained to perform reconnaissance on victim devices to gather information about the victim network. (2023-04)



* Tactic: TA0002 - Execution
  * Technique: T1210 - Exploitation of Remote Services
  * Reasons:
    * Exploit SNMP (Simple Network Management Protocol) to gain access to the target system



* Tactic: TA0011 - Command and Control
  * Technique: T1105 - Ingress Tool Transfer
  * Reasons:
    * Deploy malware to extract information from the target system. (2023-04)
  * Technique: T1071 - Application Layer Protocol
    * SubTechnique: T1071.002 - Application Layer Protocol: File Transfer Protocols
    * Reasons:
      * Use TFTP (Trivial File Transfer Protocol) for communication and data exfiltration. (2023-04)
