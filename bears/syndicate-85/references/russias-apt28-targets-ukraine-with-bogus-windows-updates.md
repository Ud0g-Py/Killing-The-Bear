---
description: '2023-05-05'
---

# Russia's APT28 Targets Ukraine With Bogus Windows Updates

The Kremlin-backed threat group APT28 is flooding Ukrainian government agencies with email messages about bogus**Windows updates**in the hope of dropping malware that will exfiltrate system data.According to the Computer Emergency Response Team of Ukraine (CERT-UA), the advanced persistent threat (APT) group – which also is known as**Fancy Bear**, Strontium, and Sofacy, among other names – [sent emails](https://cert.gov.ua/article/4492467) throughout April with "Windows Update" in the subject line.

The messages appeared to have been sent by system administrators of government agencies.

"E-mail addresses of senders created on the public service '@outlook.com' can be formed using the employee's real surname and initials," CERT-UA wrote in a brief online note.

Within the messages are instructions written in Ukrainian to update the Microsoft OS "against hacker attacks" and illustrations showing how to launch a command line and execute a PowerShell command.

Executing the command simulates a Windows update but actually downloads and executes a PowerShell script that collects basic system information about using such commands as "tasklist" and "systeminfo". The information is then sent via a HTTP request to Mocky – a service that mocks APIs to help developers test apps.

CERT-UA has advised government agencies to restrict users from running PowerShell and to monitor network connections to Mocky.

The notorious APT28 group has been around since 2008. The US Cybersecurity and Infrastructure Security Agency (CISA), and security vendors such as Secureworks and Google-owned Mandiant link it to Russia's GRU intelligence agency.

Fancy Bear has in the past targeted government and military agencies and private entities in the US, Western Europe, and South America, using phishing and similar scams. In 2018, the US Justice Department charged seven GRU operatives for their roles in APT28 attacks.

Two years later, the US and UK accused APT28 and another Russian-linked group, APT29 – or

**Cozy Bear**

– of trying to steal information about COVID-19 vaccines.

More recently, APT28 has been active in Ukraine on the cyber front of Russia's illegal invasion of its neighbor. Malwarebytes, Google, and CERT-UA found the group was behind a scheme to drop info-stealing malware using the [Follina exploit](https://www.malwarebytes.com/blog/threat-intelligence/2022/06/russias-apt28-uses-fear-of-nuclear-war-to-spread-follina-docs-in-ukraine).

US and UK agencies said in an April 2023 joint statement APT28 [exploited](https://www.cisa.gov/sites/default/files/2023-04/apt28-exploits-known-vulnerability-to-carry-out-reconnaissance-and-deploy-malware-on-cisco-routers-uk.pdf) an [older flaw](https://www.theregister.com/2023/04/18/uk\_us\_apt28\_cisco\_routers/) in unpatched Cisco routers to steal network data from US and European governments as well as about 250 Ukrainian network devices.

A Ukrainian hacktivist group called Kiber Sprotyv ("Cyber Resistance") last month [reportedly](https://cybernews.com/cyber-war/dnc-hacker-exposed-fancy-bear/) countered APT 28 by accessing the personal accounts of Sergey Alexandrovich Morgachev, a member of the GRU and alleged head of the hacking group. ®
