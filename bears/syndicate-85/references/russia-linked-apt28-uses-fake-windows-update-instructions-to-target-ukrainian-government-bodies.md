---
description: '2023-05-01'
---

# Russia-linked APT28 Uses Fake Windows Update Instructions to Target Ukrainian Government Bodies

### CERT-UA warns of a spear-phishing campaign conducted by APT28 group targeting**Ukrainian government**

### bodies with fake ‘**Windows Update**

### ’ guides.

Russia-linked [APT28](https://securityaffairs.com/145007/apt/apt28-targets-cisco-networking-equipment.html) group is targeting Ukrainian government bodies with fake ‘Windows Update’ guides, Computer Emergency Response Team of Ukraine (CERT-UA) warns.The [APT28](https://securityaffairs.co/wordpress/76922/intelligence/apt28-back-espionage.html) group (aka [**Fancy Bear**](https://securityaffairs.co/wordpress/72072/intelligence/fancy-bear-abuses-lojack.html), [Pawn Storm](https://securityaffairs.co/wordpress/53360/cyber-crime/pawn-storm-apt-0days.html), [Sofacy Group](https://securityaffairs.co/wordpress/73299/hacking/sofacy-apt-attacks.html), [**Sednit**](https://securityaffairs.co/wordpress/73299/hacking/sofacy-apt-attacks.html)

, and [STRONTIUM](https://securityaffairs.co/wordpress/74582/intelligence/microsoft-russia-attacks.html)) has been active since at least 2007 and it has targeted governments, militaries, and security organizations worldwide. The group was involved also in the string of attacks that targeted [2016 Presidential election](https://securityaffairs.co/wordpress/74434/hacking/russian-intelligence-indictment.html).

The group operates out of military unity 26165 of the Russian General Staff Main Intelligence Directorate (GRU) 85th Main Special Service Center (GTsSS).

[Most of the APT28s’ campaigns](https://securityaffairs.co/123104/apt/apt28-gmail-users-attacks.html) leveraged spear-phishing and malware-based attacks.

CERT-UA observed the campaign in April 2023, the malicious e-mails with the subject “Windows Update” were crafted to appear as sent by system administrators of departments of multiple government bodies. The threat actors sent the messages from e-mail addresses created on the public service “@outlook.com.”

_“During April 2023, the government computer emergency response team of Ukraine CERT-UA recorded cases of the distribution of e-mails with the subject “Windows Update” among government bodies of Ukraine, sent, apparently, on behalf of system administrators of departments. At the same time, e-mail addresses of senders created on the public service “@outlook.com” can be formed using the employee’s real surname and initials.” reads the_ [_alert_](https://cert.gov.ua/article/4492467) _published by CERT-UA. “The sample letter contains “instructions” in Ukrainian for “updates to protect against hacker attacks”, as well as graphical images of the process of launching a command line and executing a PowerShell command.”_

The attackers used @outlook.com email addresses using real employee names that were previously obtained in a reconnaissance phase.

The content of the messages attempts to trick recipients into launching a command line and executing a PowerShell command.

Upon executing the command, it downloads a PowerShell script on the computer that simulates a Windows updating process while downloading another PowerShell script in the background.

This second-stage payload abuses the ‘tasklist’ and ‘systeminfo’ commands to gather system information and send them to a Mocky service API via an HTTP request.

_“The mentioned command will download a PowerShell script that, simulating the process of updating the operating system, will download and execute the following PowerShell script designed to collect basic information about the computer using the “tasklist”, “systeminfo” commands, and send the received results using HTTP request to the Mocky service API.” continues the alert._

The CERT-UA recommends restricting the ability of users to launch PowerShell and monitor network connections to the Mocky service API.



CERT-UA also provided Indicators of Compromise for this campaign.

Recently, UK and US agencies are [**warned**](https://securityaffairs.com/145007/apt/apt28-targets-cisco-networking-equipment.html) of the APT28 group exploiting vulnerabilities in Cisco networking equipment.

The [Russia-linked](https://securityaffairs.com/136358/apt/apt28-powerpoint-mouseover-technique.html) APT group accesses unpatched Cisco routers to deploy malware exploiting the not patched&#x20;

[**CVE-2017-6742**](https://nvd.nist.gov/vuln/detail/CVE-2017-6742)

&#x20;vulnerability (CVSS score: 8.8), states a joint report published by the UK National Cyber Security Centre ([NCSC](https://www.ncsc.gov.uk/)), the US National Security Agency ([NSA](https://www.nsa.gov/)), US Cybersecurity and Infrastructure Security Agency ([CISA](https://www.cisa.gov/)) and US Federal Bureau of Investigation ([FBI](https://www.fbi.gov/)).

The joint advisory provides detailed info on tactics, techniques, and procedures (TTPs) associated with APT28’s attacks conducted in 2021 that exploited the flaw in Cisco routers.3-05
