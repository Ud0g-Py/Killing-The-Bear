---
description: '2023-05-04'
---

# Emulating Recent Activity from the Russian Adversary Nobelium / APT29

Since at least February 2023,**Nobelium**

has been [observed](https://blogs.blackberry.com/en/2023/03/nobelium-targets-eu-governments-assisting-ukraine) conducting spear-phishing campaigns against European Union (EU) governments in an attempt to gather intelligence on countries who support Ukraine in the ongoing Russia-Ukraine war.

During these attacks, Nobelium weaponized a Microsoft Word document using information related to a recent visit between the Poland’s Ministry of Foreign Affairs and the United States. The document included a link that redirected victims to a series of compromised websites delivering malicious HTML files that acted as droppers.

Nobelium is a politically motivated state-sponsored adversary suspected of operating on behalf of the Russian Foreign Intelligence Service (SVR). This adversary, which has been active since at least 2019, has targeted government organizations, non-governmental organizations (NGOs), think tanks, and technology service providers, primarily in the United States and Europe.

This adversary first came to light during the supply chain attack against SolarWinds, an attack that began in September 2019 and was discovered over a year later by FireEye. This attack impacted several government agencies and technology companies in the U.S. and around the world.

Nobelium is often identified in open-source reporting as a continuation of the historical adversary

**APT29**

who has been active since at least 2013. While sharing some of the same tactics, techniques, and procedures (TTPs), the malware and infrastructure was completely new emerging after an extended hiatus of APT29 activity. Multiple security vendors continue to use Nobelium and APT29 as interchangeable aliases.

AttackIQ has released a new attack graph emulating the activity conducted by Nobelium to help customers validate their security controls and their ability to defend against this threat.

Validating the performance of your security program against these behaviors is vital to reducing risk. By using this new attack graph in the AttackIQ Security Optimization Platform, security teams will be able to:

* Evaluate the performance of security controls against an active Russian threat.
* Assess security posture against a sophisticated adversary such as Nobelium who has targeted both the private and public sector worldwide.
* Continually validate detection and prevention effectiveness against an evolving actor who continues to be successful in achieving their goals.

### Nobelium – 2023-02 – Reconnaissance Campaign Against European Governments

[![Nobelium / APT29 Attack Graph](https://www.attackiq.com/wp-content/uploads/2023/05/00-attack-graph.png)(Click for Larger)](https://www.attackiq.com/wp-content/uploads/2023/05/00-attack-graph.png)

This emulation is primarily based on a [report](https://blogs.blackberry.com/en/2023/03/nobelium-targets-eu-governments-assisting-ukraine) published by BlackBerry in March 2023 and seeks to emulate a Nobelium infection chain that has includes some consistency over the years. They have several iterations of these initial stages, with one of the first observed and [reported](https://www.microsoft.com/en-us/security/blog/2021/05/28/breaking-down-nobeliums-latest-early-stage-toolset/) by Microsoft in May 2021.

[![Nobelium / APT29 Stage 1](https://www.attackiq.com/wp-content/uploads/2023/05/01-stage-1.png)(Click for Larger)](https://www.attackiq.com/wp-content/uploads/2023/05/01-stage-1.png)

The first stage of this attack begins with downloading and saving of a disc image (.ISO) file used by Nobelium as an attachment in their spearphishing messages. These files are automatically mounted by Windows when opened by the recipient exposing malicious content inside. These files drop a shortcut (LNK) file used to execute

[**GraphicalNeutrino**](https://mrtiepolo.medium.com/sophisticated-apt29-campaign-abuses-notion-api-to-target-the-european-commission-200188059f58)

, a DLL file, using Window’s native RunDLL32 utility.

**Ingress Tool Transfer (**[**T1105**](https://attack.mitre.org/techniques/T1105)**):** This scenario downloads to memory and saves to disk in two separate scenarios known malicious samples. These actions are split to test both network and endpoint controls and their ability to prevent malware.

**Subvert Trust Controls: Mark-of-the-Web Bypass (**[**T1553.005**](https://attack.mitre.org/techniques/T1553/005/)**):** This scenario bypasses MOTW by downloading and mounting an ISO image on the system to execute the payload contained inside.

**System Binary Proxy Execution: Rundll32 (**[**T1218.011**](https://attack.mitre.org/techniques/T1218/011)**):** `RunDll32` is a native system utility that can be used to execute DLL files and call a specific export inside the file. This scenario executes `RunDll32` with an AttackIQ DLL and calls an export to mimic previously reported malicious activity.

[![Nobelium / APT29 Stage 2](https://www.attackiq.com/wp-content/uploads/2023/05/02-stage-2.png)(Click for Larger)](https://www.attackiq.com/wp-content/uploads/2023/05/02-stage-2.png)

The second stage of the attack starts with obtaining persistence through the use of the Windows registry. Subsequently, system profiling is initiated where GraphicalNeutrino obtains information about the logged in user, local system, the network configuration. This stage culminates with the initial command and control communications that leveraged the API of a legitimate 3rd party service.

**Logon Autostart Execution: Registry Run Keys (**[**T1547.001**](https://attack.mitre.org/techniques/T1547/001)**):** This scenario sets the `HKLM\Software\Microsoft\Windows\CurrentVersion\Run` registry keys that Windows uses to identify what applications should be run at system startup.

**System Owner/User Discovery (**[**T1033**](https://attack.mitre.org/techniques/T1033/)**):** Executes the native `hostname` and `whoami` commands to receive details of the running user account.

**System Network Configuration Discovery (**[**T1016**](https://attack.mitre.org/techniques/T1016)**):** Native Window’s commands like `route`, `ipconfig`, and `net use` are executed to collect details about the infected host and network shares.

**Application Layer Protocol: Web Protocols (**[**T1071.001**](https://attack.mitre.org/techniques/T1071/001)**):** This scenario emulates the HTTP requests made by the GraphicalNeutrino malware by making HTTP POST and PATCH requests to an AttackIQ server that mimics the URL format and data sent during a real infection.

[![Nobelium / APT29 Stage 3](https://www.attackiq.com/wp-content/uploads/2023/05/03-stage-3.png)(Click for Larger)](https://www.attackiq.com/wp-content/uploads/2023/05/03-stage-3.png)

The final stage starts with the delivery of the actor’s second stage dropper known as

**NativeZone**. This malware also achieves persistence through another Registry Run Key and executes the final stage via RunDLL32. The final payload in this attack, known as**VaporRage**

, will perform a new communication with the adversary’s infrastructure via HTTP GET requests, in order to obtain commands and download additional payloads to achieve any final objectives.

#### Detection and Mitigation Opportunities

With so many different techniques being used by threat actors, it can be difficult to know which to prioritize for prevention and detection assessment. AttackIQ recommends first focusing on the following techniques:

**1. System Binary Proxy Execution: Rundll32 (**[**T1218.011**](https://attack.mitre.org/techniques/T1218/011)**)**

Nobelium used DLL files for many of their malware payloads and leveraged a native Windows utility to execute them.

The primary native method for executing these files is to call the `RunDll32` tool and pass along the path and export function to be executed.

**1a. Detection**

While this tool is commonly used by legitimate applications there are behaviors related to their execution that can stand out in your process logs. Searching for files that are being executed from temporary directories, that don’t have the standard .dll file extension, or call strange looking export names can stand out from regular user behavior.

`Process Name == (rundll32.exe OR regsvr32.exe)`\
`Command Line CONTAINS (‘TEMP’ OR ‘.png’ OR ‘Roaming’ OR ‘%APPDATA%’)`\
`Current Directory CONTAINS (‘D:\’ OR ‘E:\’)`

**1b. Mitigation**

* [M1050 – Exploit Protection](https://attack.mitre.org/mitigations/M1050/)

**2. Logon Autostart Execution: Registry Run Keys (**[**T1547.001**](https://attack.mitre.org/techniques/T1547/001)**):**

Preventing an actor from maintaining a foothold in your environment should always be one of the top priorities. In both campaigns, Registry Run keys were used to keep many of their malware families running after a reboot. The actor’s abused their access over multiple days and a simple reboot would have prevented their actions if the persistence couldn’t be established.

**2a. Detection**

Using a SIEM or EDR Platform to see modifications to the Run and RunOnce keys will alert when unauthorized users or software makes modifications to the keys that allow programs to run are startup.

`Process Name == reg.exe`\
`Command Line Contains (“ADD” AND “\CurrentVersion\Run”)`

**2b. Mitigation**

MITRE ATT\&CK does not have any direct mitigations as this is abusing legitimate Windows functionality. They recommend monitoring registry changes and process execution that may attempt to add these keys.
