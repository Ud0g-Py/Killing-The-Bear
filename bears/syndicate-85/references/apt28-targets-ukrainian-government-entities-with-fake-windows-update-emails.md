---
description: '2023-05-01'
---

# APT28 Targets Ukrainian Government Entities with Fake "Windows Update" Emails

The Computer Emergency Response Team of Ukraine (CERT-UA) has warned of cyber attacks perpetrated by Russian nation-state hackers targeting various government bodies in the country.

The agency [attributed](https://cert.gov.ua/article/4492467) the phishing campaign to APT28, which is also known by the names Fancy Bear, Forest Blizzard, FROZENLAKE,

**Iron Twilight**,**Sednit**

, and Sofacy.

The email messages come with the subject line "

**Windows Update**

" and purportedly contain instructions in the Ukrainian language to run a PowerShell command under the pretext of security updates.

Running the script loads and executes a next-stage PowerShell script that's designed to collect basic system information through commands like [tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist) and [systeminfo](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/systeminfo), and exfiltrate the details via an HTTP request to a [Mocky API](https://designer.mocky.io/design).

To trick the targets into running the command, the emails impersonated system administrators of the targeted government entities using fake Microsoft Outlook email accounts created with the employees' real names and initials.

CERT-UA is recommending that organizations restrict users' ability to run PowerShell scripts and monitor network connections to the Mocky API.

The disclosure comes weeks after the APT28 was tied to attacks [exploiting now-patched security flaws](https://thehackernews.com/2023/04/us-and-uk-warn-of-russian-hackers.html) in networking equipment to conduct reconnaissance and deploy malware against select targets.

Google's Threat Analysis Group (TAG), in an [advisory](https://thehackernews.com/2023/04/google-tag-warns-of-russian-hackers.html) published last month, detailed a credential harvesting operation carried out by the threat actor to redirect visitors of

**Ukrainian government**

websites to phishing domains.

Russian-based hacking crews have also been linked to the exploitation of a critical privilege escalation flaw in Microsoft Outlook (

[**CVE-2023-23397**](https://thehackernews.com/2023/03/microsoft-warns-of-stealthy-outlook.html)

, CVSS score: 9.8) in intrusions directed against the government, transportation, energy, and military sectors in Europe.

UPCOMING WEBINAR

Learn to Stop Ransomware with Real-Time Protection

Join our webinar and learn how to stop ransomware attacks in their tracks with real-time MFA and service account protection.

[Save My Seat!](https://thn.news/silver-web-inside)

The development also comes as Fortinet FortiGuard Labs [uncovered](https://www.fortinet.com/blog/threat-research/malware-disguised-as-document-ukraine-energoatom-delivers-havoc-demon-backdoor) a multi-stage phishing attack that leverages a macro-laced Word document supposedly from Ukraine's Energoatom as a lure to deliver the open source [Havoc post-exploitation framework](https://thehackernews.com/2023/02/threat-actors-adopt-havoc-framework-for.html).

"It remains highly likely that Russian intelligence, military, and law enforcement services have a longstanding, tacit understanding with cybercriminal threat actors," cybersecurity firm Recorded Future [said](https://www.recordedfuture.com/dark-covenant-2-cybercrime-russian-state-war-ukraine) in a report earlier this year.

"In some cases, it is almost certain that these agencies maintain an established and systematic relationship with cybercriminal threat actors, either by indirect collaboration or via recruitment."

