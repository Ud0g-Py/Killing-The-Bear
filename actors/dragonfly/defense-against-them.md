---
cover: ../../.gitbook/assets/energeticbear.webp
coverY: 115.34482758620689
---

# Defense Against Them

* Follow Microsoft’s [guidance](https://support.microsoft.com/en-us/help/4557222/how-to-manage-the-changes-in-netlogon-secure-channel-connections-assoc) on monitoring logs for activity related to the Netlogon vulnerability, CVE-2020-1472.
* Prevent external communication of all versions of SMB and related protocols at the network boundary by blocking Transmission Control Protocol (TCP) ports 139 and 445 and User Datagram Protocol (UDP) port 137. See the CISA publication on [SMB Security Best Practices](https://us-cert.cisa.gov/ncas/current-activity/2017/01/16/SMB-Security-Best-Practices) for more information.
* Implement the prevention, detection, and mitigation strategies outlined in CISA Alert [TA15-314A – Compromised Web Servers and Web Shells – Threat Awareness and Guidance](https://us-cert.cisa.gov/ncas/alerts/TA15-314A) and National Security Agency Cybersecurity Information Sheet [U/OO/134094-20 – Detect and Prevent Web Shells Malware](https://www.nsa.gov/News-Features/News-Stories/Article-View/Article/2159419/detect-prevent-cyber-attackers-from-exploiting-web-servers-via-web-shell-malware/).
* Isolate external facing services in a network demilitarized zone (DMZ) since they are more exposed to malicious activity; enable robust logging, and monitor the logs for signs of compromise.
* Establish a training mechanism to inform end users on proper email and web usage, highlighting current information and analysis and including common indicators of phishing. End users should have clear instructions on how to report unusual or suspicious emails.
* Implement application controls to only allow execution from specified application directories. System administrators may implement this through Microsoft Software Restriction Policy, AppLocker, or similar software. Safe defaults allow applications to run from PROGRAMFILES, PROGRAMFILES(X86), and WINDOWS folders. All other locations should be disallowed unless an exception is granted.
* Block Remote Desktop Protocol (RDP) connections originating from untrusted external addresses unless an exception exists; routinely review exceptions on a regular basis for validity.
