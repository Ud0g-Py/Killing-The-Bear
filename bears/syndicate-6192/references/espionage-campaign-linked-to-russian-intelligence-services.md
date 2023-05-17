---
description: '2023-04-13'
---

# Espionage campaign linked to Russian intelligence services

The Military Counterintelligence Service and the CERT Polska team (CERT.PL) observed a widespread espionage campaign linked to Russian intelligence services

![trzy loga english](https://www.gov.pl/photo/format/17b99cf0-27f9-4687-b5ca-2b18205208f7/resolution/1920x810)

#### Espionage campaign linked to Russian intelligence services

The Military Counterintelligence Service and the CERT Polska team (CERT.PL) observed a widespread espionage campaign **linked to Russian intelligence services**, aimed at collecting information from foreign ministries and diplomatic entities. Most of the identified targets of the campaign are located in NATO member states, the European Union and, to a lesser extent, in Africa.

Many elements of the observed campaign -- the infrastructure, the techniques used and the tools - overlap, in part or in full, with activity described in the past, referred to by Microsoft as "NOBELIUM" and by Mandiant as "APT29". The actor behind them has been linked to, among other things, a campaign called "SOLARWINDS"^1^ and the tools "SUNBURST", "ENVYSCOUT"^2. 3.^ and "BOOMBOX"^4^, as well as numerous other espionage campaigns^5^.

The activities described here differ from the previous ones in the use of software unique to this campaign and not previously described publicly. New tools^6^ were used at the same time and independently of each other, or replacing those whose effectiveness had declined, allowing the actor to maintain continues, high operational tempo.

At the time of publication of the report, the campaign is still ongoing and in development. The Military Counterintelligence Service and CERT.PL recommend all entities which may be in the area of interest of the actor to implement mechanisms aimed at improving the security of IT Security systems in use and increasing the detection of attacks. Examples of configuration changes and detection mechanisms are proposed in the recommendations.

The aim of publishing the advisory is to disrupt the ongoing espionage campaign, impose additional cost of operations against allied nations and enable the detection, analysis and tracking of the activity by affected parties and the wider cyber security industry.

#### The course of the observed campaigns

In all observed cases, the actor utilised spear phishing techniques. Emails impersonating embassies of European countries were sent to selected personnel at diplomatic posts. The correspondence contained an invitation to a meeting or to work together on documents. In the body of the message or in an attached PDF document, a link was included purportedly directing to the ambassador's calendar, meeting details or a downloadable file.&#x20;

![Example of an email](https://www.gov.pl/photo/76ff84a2-8f4a-4517-abdc-d1b4eba0c671)

Figure 1. Example of an email impersonating the Polish embassy and urging the addressee to click on a malicious link.\
The link directed to a compromised website that contained the actor's signature script, publicly referred to as "ENVYSCOUT". It utilises the HTML Smuggling technique -- whereby a malicious file placed on the page is decoded using JavaScript when the page is opened and then downloaded on the victim's device. This makes the malicious file more difficult to detect on the server side where it is stored. The web page also displayed an information intended to reassure the victim that they had downloaded the correct attachment.

In the course of the described campaign, three different versions of the ENVYSCOUT tool were observed, progressively adding new mechanisms to hinder analysis.

![A website](https://www.gov.pl/photo/1d6ed6df-90c2-4d57-8c2e-4ca959e53700)

Figure 2. A website impersonating the Polish embassy suggesting a downloadable calendar\
Campaigns observed in the past linked to "NOBELIUM" and "APT29" used .ZIP or .ISO files to deliver the malware. During the campaign described above, .IMG files were also used in addition to the aforementioned file formats. ISO and IMG disk images, on Windows computers, are automatically mounted in the file system when opened, which causes their contents to be displayed in Windows Explorer. In addition, they do not carry the so-called _mark-of-the-web_, i.e. the user will not be warned that the files were downloaded from the Internet.\
The actor used various techniques to get the user to launch the malware. One of them was a Windows shortcut (LNK) file pretending to be a document but actually running a hidden DLL library with the actor's tools. The _DLL Sideloading_ technique was also observed, using a signed executable file to load and execute code contained in a hidden DLL library by placing it in the same directory, under a name chosen according to the entries in the import table. At a later stage of the campaign, the name of the executable file contained many spaces to make the exe extension difficult to spot.

![Windows Explorer](https://www.gov.pl/photo/6bd56ba0-3e74-4360-afa5-8b68e12365e1)

Figure 3. View after the victim starts-up an image file with the default Windows Explorer settings.

#### Tools used during the campaign

The actor used various tools at different stages of the described campaign. All those listed below are unique to the set of activities described. A detailed technical analysis of each is included in separate documents:

1. [SNOWYAMBER](https://www.gov.pl/attachment/ee91f24d-3e67-436d-aa50-7fa56acf789d) -- a tool first used in October 2022, abusing the Notion^7^ service to communicate and download further malicious files. Two versions of this tool have been observed.
2. [HALFRIG](https://www.gov.pl/attachment/64193e8d-05e2-4cbf-bb4c-5f58da21fefb) -- used for the first time in February 2023. This tool is distinguished from the others by the embedded code that runs the COBALT STRIKE tool.
3. [QUARTERRIG](https://www.gov.pl/attachment/6f51bb1a-3ad2-461c-a16d-408915a56f77) -- a tool first used in March 2023, sharing part of the code with HALFRIG. Two versions of this tool were observed.

The first version of the SNOWYAMBER tool was publicly described by Recorded Future, among others^8^. **A modified version of the SNOWYAMBER tool, the HALFRIG tool and the QUATERRIG tool have not previously been described publicly.**

![Timeline](https://www.gov.pl/photo/f97cc54b-7c80-414b-a6d2-983ae0fdeeb3)

Figure 4. Timeline illustrating the observed actions of the actor\
The SNOWYAMBER and QUARTERRIG tools were used as so-called downloaders. Both tools sent the IP address as well as the computer and user name to the actor. They were used to assess whether the victim was of interest to the actor and whether it was a malware analysis environment. If the infected workstation passed manual verification, the aforementioned downloaders were used to deliver and start-up the commercial tools COBALT STRIKE or BRUTE RATEL. HALFRIG, on the other hand, works as a so-called loader -- it contains the COBALT STRIKE payload and runs it automatically.

![the actor's tool delivery course](https://www.gov.pl/photo/83244748-9f0d-44af-8169-eadc4e72fe92)

Figure 5. Illustration of the actor's tool delivery course\
Despite the observed changes in tools, many of the elements of the campaign are repeatable. These include:

1. The way the infrastructure is built. The actor behind the espionage campaign prefers to use vulnerable websites belonging to random entities.
2. Email theme. All acquired emails used in the campaigns used the theme of correspondence between diplomatic entities.
3. The use of a tool publicly referred to as ENVYSCOUT. This script has been used by the actor since at least 2021^9^. Modifications to the tool's code were observed during the campaign, but they did not significantly affect its functionality.
4. A link to the ENVYSCOUT tool was provided to the victim in the form of a link embedded in the body of the email or in the body of an attached PDF file.
5. Use of ISO and IMG disc images.
6. Use of a technique called "DLL Sideloading" that uses a non-malicious, digitally signed executable file to start-up the actor's tools.
7. Use of commercial tools COBALT STRIKE and BRUTE RATEL.

#### Recommendations

The Military Counterintelligence Service and CERT.PL **strongly** recommend that all entities that may be in the actor's area of interest implement configuration changes to disrupt the delivery mechanism that was used in the described campaign. Sectors that should **particularly** consider implementing the recommendations are:

1. Government entities;
2. Diplomatic entities, foreign ministries, embassies, diplomatic staff and those working in international entities;
3. International organisations;
4. Non-Governmental organisations.

The following configuration changes can be used to disrupt the malware delivery mechanism used in the described campaign:

1. Blocking the ability to mount disk images on the file system. Most users doing office work have no need to download and use ISO or IMG files.
2. Monitoring of the mounting of disk image files by users with administrator roles.
3. Enabling and configuring _Attack Surface Reduction Rules^10^_.
4. Configuring Software Restriction Policy and blocking the possibility of starting-up executable files from unusual locations (in particular: temporary directories, %localappdata% and subdirectories, external media^11^).

We also include a collection of all observed indicators of compromise (IoCs) related to the campaign described, and we recommend to verify the system and network logs collected for their occurrence.

\###\
**Attachments**

* [**SNOWYAMBER tool analysis**](https://www.gov.pl/attachment/ee91f24d-3e67-436d-aa50-7fa56acf789d)
* [**HALFRIG tool analysis**](https://www.gov.pl/attachment/64193e8d-05e2-4cbf-bb4c-5f58da21fefb)
* [**QUARTERRIG tool analysis**](https://www.gov.pl/attachment/6f51bb1a-3ad2-461c-a16d-408915a56f77)
* [**IoC related to the campaing**](https://www.gov.pl/attachment/6e085a2c-ac05-4b62-9423-5d6e9ef730bf)

***

^1 ^https://www.mandiant.com/resources/blog/unc2452-merged-into-apt29\
^2^ https://www.microsoft.com/en-us/security/blog/2021/05/27/new-sophisticated-email-based-attack-from-nobelium/,\
https://www.microsoft.com/en-us/security/blog/2021/05/28/breaking-down-nobeliums-latest-early-stage-toolset/\
^3^ https://www.mandiant.com/resources/blog/tracking-apt29-phishing-campaigns\
^4 ^Terminology taken from the Microsoft MSTIC team's publicly available analysis:\
https://www.microsoft.com/en-us/security/blog/2021/05/27/new-sophisticated-email-based-attack-from-nobelium/.\
^5^ https://media.defense.gov/2021/Apr/15/2002621240/-1/-1/0/CSA\_SVR\_TARGETS\_US\_ALLIES\_UOO13234021.PDF,\
https://www.ncsc.gov.uk/files/Advisory%20Further%20TTPs%20associated%20with%20SVR%20cyber%20actors.pdf,\
https://www.gov.uk/government/news/russia-uk-and-us-expose-global-campaigns-of-malign-activity-by-russian-intelligence-services,\
https://www.ncsc.gov.uk/news/advisory-apt29-targets-covid-19-vaccine-development\
^6^ The term "tools" is used in a broad sense and includes file delivery scripts, "loader", "stager" and "dropper" software\
^7^ https://www.notion.so/\
^8^ https://go.recordedfuture.com/hubfs/reports/cta-2023-0127.pdf\
^9^ https://microsoft.com/en-us/security/blog/2021/05/28/breaking-down-nobeliums-latest-early-stage-toolset/\
^10^ https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/attack-surface-reduction\
^11 ^For example:\
C:\Windows\Temp\*.exe\
C:\Windows\Temp\*\*.exe\
%USERPROFILE%\AppData\Local\*.exe\
%USERPROFILE%\AppData\Local\*\*.exe\
%USERPROFILE%\AppData\Roaming\*.exe\
%USERPROFILE%\AppData\Roaming\*\*.exe202

MITRE ATT\&CK: \[MITRE ATT\&CK] T1583.003 - Acquire Infrastructure: Virtual Private Server | \[MITRE ATT\&CK] T1583.006 - Acquire Infrastructure: Web Services | \[MITRE ATT\&CK] T1584 - Compromise Infrastructure | \[MITRE ATT\&CK] T1566 - Phishing | \[MITRE ATT\&CK] T1566.001 - Phishing: Spearphishing Attachment | \[MITRE ATT\&CK] T1566.002 - Phishing: Spearphishing Link | \[MITRE ATT\&CK] T1204 - User Execution | \[MITRE ATT\&CK] T1204.002 - User Execution: Malicious File | \[MITRE ATT\&CK] T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder | \[MITRE ATT\&CK] T1574.001 - Hijack Execution Flow: Dll Search Order Hijacking | \[MITRE ATT\&CK] T1574.002 - Hijack Execution Flow: Dll Side-Loading | \[MITRE ATT\&CK] T1027.006 - Obfuscated Files or Information: Html Smuggling | \[MITRE ATT\&CK] T1140 - Deobfuscate/Decode Files Or Information | \[MITRE ATT\&CK] T1553.005 - Subvert Trust Controls: Mark-Of-The-Web Bypass | \[MITRE ATT\&CK] T1574.001 - Hijack Execution Flow: Dll Search Order Hijacking | \[MITRE ATT\&CK] T1574.002 - Hijack Execution Flow: Dll Side-Loading | \[MITRE ATT\&CK] T1102 - Web Service | \[MITRE ATT\&CK] T1102.003 - Web Service: One-Way Communication

