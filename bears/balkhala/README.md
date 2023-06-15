# Balkhala

<figure><img src="../../.gitbook/assets/bear_Balkhala_cover.png" alt=""><figcaption><p>Balkhala</p></figcaption></figure>

Balkhala is a network operator that predominantly uses living-off-the-land techniques to infiltrate networks, perform lateral movement, collect credentials, and deploy defense evasion and persistence mechanisms. Unlike other Russian-affiliated groups focusing on espionage, Balkhala's operations are often disruptive and potentially aimed at destruction, disruption, and intimidation.

The group's operational lifecycle includes exploiting servers, deploying web shells, using tunneling tools, and pivoting to internal networks for initial access. Lateral movement involves credential access through process dumping, interactive reverse shell via netcat/GOST, command execution via Impacket, disabling antivirus services, and wiping logs. Balkhala's actions on objectives include data exfiltration, deploying destructive payloads, and leaking data or conducting targeted information operations.

Balkhala exploits web servers, Confluence servers, Exchange servers, and various open-source platforms to gain initial access. They persist on target networks using commodity web shells such as P0wnyshell, reGeorg, PAS, and custom variants. The group uses various living-off-the-land techniques for privilege escalation and credential harvesting, such as dumping LSASS memory and extracting registry hives. Lateral movement is conducted using valid network credentials obtained from credential harvesting, facilitated through the Impacket framework and other tools.

Balkhala uses generic socket-based tunneling utilities for command and control (C2) and has occasionally utilized Meterpreter. The group employs anonymization services like IVPN, SurfShark, and Tor during select operations and engages in anti-forensics activities, such as extracting event logs and deleting files used during operational phases. They are also known to disable Microsoft Defender Antivirus through various means.
