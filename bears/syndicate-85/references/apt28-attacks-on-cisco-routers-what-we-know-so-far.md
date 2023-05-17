---
description: '2023-04-19'
---

# APT28 Attacks on Cisco Routers: What We Know So Far

## APT28 Attacks on**Cisco**

## Routers: What We Know So Far

Russian cyber-attack targets Cisco devices; US and European institutions hit. Read on for the details and suggested mitigations.

### Executive Summary <a href="#executive-summary" id="executive-summary"></a>

* APT28, the Russian General Staff Main Intelligence Directorate (GRU), has exploited critical vulnerabilities in poorly-maintained Cisco routers.
* The attack has affected European and US government institutions, as well as approximately 250 Ukrainian victims.
* Network administrators are urged to disable remote management, block access to ports 443 and 60443, and follow the mitigation advice provided by CISA.
* Cisco has not released software updates to address these vulnerabilities.

### What Happened? <a href="#what-happened" id="what-happened"></a>

APT28, an advanced persistent threat group, exploited critical vulnerabilities in Cisco routers (

**CVE-2023-20025**,**CVE-2023-20026**, and**CVE-2023-20118**

) to bypass authentication, execute arbitrary commands on the underlying operating system, and deploy malware on unpatched devices \[2]. These vulnerabilities affect Cisco Small Business RV016, RV042, RV042G, RV082, RV320, and RV325 routers.

### Who is affected? <a href="#who-is-affected" id="who-is-affected"></a>

The attack affected Cisco routers worldwide, including a small number of European devices, US government institutions, and approximately 250 Ukrainian victims \[1].

### Who is the attacker? <a href="#who-is-the-attacker" id="who-is-the-attacker"></a>

CISA, along with the UK National Cyber Security Centre (NCSC), the US National Security Agency (NSA), and the US Federal Bureau of Investigation (FBI), has identified APT28 as the Russian General Staff Main Intelligence Directorate (GRU) 85th Special Service Centre (GTsSS) Military Intelligence Unit 26165 \[1]. APT28 is also known as

**Fancy Bear**, STRONTIUM, Pawn Storm, the**Sednit**

Gang, and Sofacy.

### What is the mitigation? <a href="#what-is-the-mitigation" id="what-is-the-mitigation"></a>

Cisco has not released software updates to address the vulnerabilities described in this advisory. The following mitigation steps are recommended \[2]:

* Disable remote management of affected devices.
* Block access to ports 443 and 60443.
* Update routers to the latest patched software to close the**CVE-2017-6742**
* vulnerability \[1].
* Limit SNMP access to trusted hosts only or disable specific SNMP Management Information bases (MIBs) to reduce the attack surface \[1].
* Use strong, unique SNMP community strings instead of default or easy-to-guess ones \[1].
* Consider upgrading to SNMP v3, which supports encryption, to protect sensitive network information \[1].

### What should you do if you're affected? <a href="#what-should-you-do-if-youre-affected" id="what-should-you-do-if-youre-affected"></a>

Affected organizations should immediately report the incident to the appropriate authorities. UK organizations should contact the NCSC, while US organizations should reach out to CISA’s 24/7 Operations Centre at report@cisa.gov or (888) 282-0870 \[1].

### Additional context and information <a href="#additional-context-and-information" id="additional-context-and-information"></a>

APT28 is a highly skilled threat actor with a history of cyber attacks, such as the 2015 German parliament data theft and the attempted attack on the Organization for the Prohibition of Chemical Weapons (OPCW) in April 2018 \[1]. As of 2021, the group has been using commercially available code repositories and post-exploit frameworks like Empire \[1].

### Sources <a href="#sources" id="sources"></a>

\[1][https://www.cisa.gov/sites/default/files/2023-04/apt28-exploits-known-vulnerability-to-carry-out-reconnaissance-and-deploy-malware-on-cisco-routers-uk.pdf](https://www.cisa.gov/sites/default/files/2023-04/apt28-exploits-known-vulnerability-to-carry-out-reconnaissance-and-deploy-malware-on-cisco-routers-uk.pdf?ref=securityengineering.dev)

\[2][https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20170629-snmp](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20170629-snmp?ref=securityengineering.dev)
