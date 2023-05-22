---
description: '2023-05-22'
---

# Threat Actor Profile: ALPHV Ransomware Group

ALPHV is a financially-motivated threat actor that emerged in late 2021. This actor is notable for operating a Ransomware-as-a-Service (RaaS) model, primarily for its novel BlackCat ransomware. For attribution, this actor is tracked under other names that include: UNC4466, BlackCat and Noberus. Ransomware-as-a-Service or RaaS is a model of criminal extortion operations that sees a primary ransomware author sell access to its malware payload to affiliate actors who commonly carryout tasks such as target selection and exploitation. The malware author sets a percentage cut of the collected ransom from victims to the affiliates with ALPHV providing up to a generous 90% to its affiliates. ALPHV uses a combination of publicly-available and novel tools across its attack chain that’s tied together to a triple extortion tactic. The group has targeted entities in multiple industries that have included manufacturing, medical, transportation and financial in several countries that include the United States, Australia, Germany, Italy and the United Kingdom. This actor is part of a family of Russia-based ransomware syndicate groups.

Known Techniques & Capabilities

The primary payload of ALPHV is the BlackCat ransomware which is particularly unique for being developed in the Rust scripting language. This allows BlackCat to be deployed across both Windows and Linux platforms, including different Windows operating systems and distros of Linux. The BlackCat ransomware comes with anti-analysis capabilities with the utilization of the Zeroize library which clears attributable artifacts from memory. ALPHV operations are often highly targeted and this is reflected in custom configurations that have been discovered in analyzed BlackCat payload samples. BlackCat comes embedded with a variety of configurations written in JSON that the ALPHV affiliates can adjust in the post- reconnaissance phase. The ransomware can be configured to use a variety of different encryption modes. These include full file encryption, SmartPattern as well as DotPattern encryption options. Such default configurations can be overridden to address differing circumstances around the target environment. It also must be run with a 32-bit access token. Particular capabilities of both Linux and Windows variants include:

· Support for VMWare ESXi for the purpose of stopping virtual machines and deleting VM snapshots.

· Escalate privileges via the bypassing of User Account Control (UAC); Masquerade\_PEB; the exploitation of ‘CreateProcessWithLogonW’ API tracked as CVE-2016–0099.

· Using ‘fsutil’ to enable ‘remote to local’ and ‘remote to ‘remote’.

· Acquires the universally unique identifier (UUID) from the host via the Windows Management Interface Command-line (WMIC).

· Use PsExec to execute the ransomware on a remote host for expanded propagation.

**Analyst Comments:**

The targeting of VMWare ESXi within the Linux variants is an additional technique for blocking the ability of the host to recover their encrypted files from backups stored in the virtual machine snapshots. This technique was also notably employed by the BlackMatter and Conti ransomware actors in 2021. Other features of the ransomware are more common among other ransomware families such as removing shadow copies and terminating specific processes like Microsoft Exchange and Office applications. If you’re maintaining recovery backups in a virtual environment, it’s strongly recommended to keep such environments insulated and disconnected from your network.

As mentioned, ALPHV uses a triple extortion technique for its targets which involves the exfiltration of sensitive information but with the addition of threatening the victim with distributed denial of service (DDoS) attacks if the demanded ransom is not paid. Exmatter is one of the toosl used by ALPHV for the exfiltration of data. At the start of an operation, ALPHV affiliates will conduct reconnaissance to profile the target, allowing them to better configure follow-on tools and BlackCat.

ALPHV affiliates commonly use phishing, open ports, known vulnerabilities and stolen credentials for initial access which are common initial vectors among other ransomware groups. Stolen credentials become another component to ALPHV activity during the later privilege escalation and lateral movement phases. A recently observed ALPHV operation saw the exploitation of vulnerabilities affecting the Veritas Backup Exec installations that included CVE-2021–27876, CVE-2021–27877 and CVE-2021–27878. Additionally in this early stage, scanning tools such as Advanced IP Scanner by Famatech and ADRecon are downloaded onto the victim’s environment. ADRecon is a tool that allows ALPHV to effectively map an Active Directory environment, if present, and provides this back to the attacker in a generated report.

**Analyst Comments:**

Organizations can take proactive steps to mitigate against these attack vectors by closely monitoring traffic coming from Remote Desktop Protocol (RDP) and Virtual Private Network (VPN) services. In other cases, turning off RDP altogether would be recommended if the port is not in use. Likewise, having strong password and authentication policies in place are critical to preventing unauthorized access or limiting lateral movement. This includes complex passwords with regular enforced changes and the implementation of multi-factor authentication. Finally, organizations should consistently check their systems to make sure the latest patches have been applied. ALPHV is noted for having a thorough information gathering phase which allows them to create a clearer profile of a target organization and potential initial vectors they can exploit. Metasploit is another common tool used for legitimate penetration testing that’s been used by ALPHV as well as other threat actors. Exploit kits could be further aiding ALPHV in more quickly identifying exploitable vulnerabilities.

On top of using already stolen credentials, ALPHV utilizes publicly-available credential scrapping tools within the target environment. Nanodump, Mimikatz and LaZagne allow the attacker to gather clear-text passwords. These tools are downloaded onto the target system using the Background Intelligence Transfer Service (BITS). The gathering of additional credential allows ALPHV to manipulate and escalate account privileges as well as carryout lateral movement. RDP is also a common vector point used for moving laterally (T1021.001) along with PsExec that lets ALPHV remotely execute processes. For establishing persistence, ALPHV operators will leverage legitimate Windows services (T1543.003), generating Registry run keys to execute their tools at system startup (T1547.001) and create system processes (T1543).

Outside of the obfuscation capabilities built within BlackCat, ALPHV operators also clear event logs (in Windows environments), conduct token manipulation and theft, and disable real-time monitoring tools that include Windows Defender.

Targets

ALPHV has shown little restraint in its targeting approach, and has directed its activity against organizations across nearly all industry sectors. These have included transportation services, fuel logistics, manufacturing, technology, energy, financial, medical and communications. This indiscriminate level of targeting could be a carryover trait from members of the former REvil and BlackMatter ransomware groups.

Closing Analysis

As of April 2023, ALPHV has evolved itself into one of the most prolific ransomware groups in the current threat landscape, only falling behind the Lockbit ransomware group in observed activity. Being primarily a Russia-based group, ALPHV will unlikely target organizations based in the Russian Federation or among the rest of the Commonwealth of Independent States (CIS) that make up the former Soviet Union.

The actor is also strongly suspected of being the successor to REvil, DarkSide and the BlackMatter ransomware groups. In addition to that history, the current ALPHV group is believed to have links to other Russia-based cybercrime syndicates such as FIN7 and FIN12.

While ALPHV does not have the footprint of actors like Lockbit, TA505 and TA542 (creators of Emotet), their rapid growth, innovation, operational patients and lack of restraint make them a high threat to organizations with services and interests across the globe. The novel characteristics of BlackCat and its reach are compounded by a robust anti-analysis/detection component of ALPHV campaigns. The use of a triple extortion tactic further enhances the current threat from this group. Finally, this actor represents years of learning from previous mistakes within the cybercriminal ecosphere, likely encompassing very skilled individuals. It’s highly likely this actor will continue to grow its activity through the rest of 2023.
