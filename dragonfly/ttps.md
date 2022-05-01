---
cover: ../.gitbook/assets/energeticbear.webp
coverY: 111.72413793103448
---

# TTPs

## Mitre

* Dragonfly: [https://attack.mitre.org/groups/G0035/](https://attack.mitre.org/groups/G0035/)
* Dragonfly 2.0: [https://attack.mitre.org/groups/G0074/](https://attack.mitre.org/groups/G0074/) \*\*
* Complete map:  [single MITRE ATT\&CK Mapping](https://mitre-attack.github.io/attack-navigator/#layerURL=https://raw.githubusercontent.com/scythe-io/community-threats/master/BerserkBear/BerserkBear\_ATT%26CK\_Navigator.json)&#x20;



| Tactic                | Description                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Objective             | Hold access to critical infrastructure for later use. However, we have yet to see them pull the trigger.                                                                                                                                                                                                                                                                                                            |
|  Command and Control  | <p>T1071 - Application Layer Protocol<br>T1573 - Encrypted Channel</p>                                                                                                                                                                                                                                                                                                                                              |
|  Collection           | <p>T1560 - Archive Collected Data<br>T1074.001 - Local Data Staging<br>T1005 - Data from Local System<br>T1114.002 - Remote Email Collection<br>T1056.001 - Keylogging<br>T1113 - Screen Capture</p>                                                                                                                                                                                                                |
|  Execution            | <p>T1059 - Command and Scripting Interpreter<br>T1059.001 - PowerShell<br>T1059.003 - Windows Command Shell<br>T1059.006 - Python<br>T1053.005 - Scheduled Task<br>T1204.001 - Malicious Link<br>T1204.002 - Malicious File</p>                                                                                                                                                                                     |
|  Defense Evasion      | <p>T1562.004 - Disable or Modify System Firewall<br>T1070.001 - Clear Windows Event Logs<br>T1070.004 - File Deletion<br>T1036 - Masquerading<br>T1112 - Modify Registry<br>T1027 - Obfuscated Files or Information<br>T1027.002 - Software Packing<br>T1055 - Process Injection<br>T1055.003 - Thread Execution Hijacking<br>T1221 - Template Injection<br>T1078 - Valid Accounts<br>T1497.001 - System Checks</p> |
|  Credential Access    | <p>T1110.002 - Password Cracking<br>T1555.003 - Credentials from Web Browsers<br>T1187 - Forced Authentication<br>T1003 - OS Credential Dumping<br>T1003.002 - Security Account Manager<br>T1003.003 - NTDS<br>T1003.004 - LSA Secrets</p>                                                                                                                                                                          |
|  Persistence          | <p>T1098 - Account Manipulation<br>T1547.001 - Registry Run Keys / Startup Folder<br>T1547.009 - Shortcut Modification<br>T1136.001 - Local Account<br>T1133 - External Remote Services<br>T1505.003 - Web Shell</p>                                                                                                                                                                                                |
|  Discovery            | <p>T1012 - Query Registry<br>T1016 - System Network Configuration Discovery<br>T1033 - System Owner/User Discovery<br>T1049 - System Network Connections Discovery<br>T1057 - Process Discovery<br>T1082 - System Information Discovery<br>T1083 - File and Directory Discovery<br>T1135 - Network Share Discovery</p>                                                                                              |

## Initial Access

* Drive-By Compromise \[[T1189](https://attack.mitre.org/techniques/T1189/)]
* Exploiting Public Facing Applications \[[T1190](https://attack.mitre.org/techniques/T1190/)]
* External Remote Services \[[T1133](https://attack.mitre.org/techniques/T1133/)]
* Credential Access via Brute Force \[[T1110](https://attack.mitre.org/techniques/T1110/)]
* Lateral Movement \[[TA0008](https://attack.mitre.org/tactics/TA0008/)], Persistence \[[TA0003](https://attack.mitre.org/tactics/TA0003/)], and Privilege Escalation \[[TA0004](https://attack.mitre.org/tactics/TA0004/)]
* Valid Accounts \[[T1078](https://attack.mitre.org/techniques/T1078/)]

