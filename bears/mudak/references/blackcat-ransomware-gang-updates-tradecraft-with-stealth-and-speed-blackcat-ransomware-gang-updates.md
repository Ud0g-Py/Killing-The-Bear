---
description: '2023-06-02'
---

# BlackCat ransomware gang updates tradecraft with stealth and speed BlackCat ransomware gang updates

## BlackCat ransomware gang updates tradecraft with stealth and speed <a href="#blackcat-ransomware-gang-updates-tradecraft-with-stealth-and-speed" id="blackcat-ransomware-gang-updates-tradecraft-with-stealth-and-speed"></a>

BlackCat ransomware gang updates tradecraft with stealth and speed Simon Hendery BlackCat ransomware gang updates malware with stealth and speed

Ransomware group BlackCat (also known as ALPHV) has risen to prominence over the past 18 months and new research details how a retooling of its tradecraft earlier this year made it an even more powerful threat.

“BlackCat has become known as a highly formidable and innovative ransomware operation since its debut in November 2021,” researchers from IBM Security X-Force said in an analysis of the group and its evolving malware posted Tuesday.

“BlackCat has consistently been listed among the top 10 most active ransomware groups by multiple research entities and was linked in an April 2022 FBI advisory to now-defunct BlackMatter/DarkSide ransomware.” Despicable track record

The Russia-based group and its affiliates have attempted extortions around the world and across multiple industries, sometimes putting pressure on victims by publishing sensitive stolen data including financial and medical information.

In March in released photos of topless female breast cancer patients at the Lehigh Valley Health Network after the organization refused to pay a $1.5 million ransom following an attack in February.

Since then, BlackCat victims have included Western Digital, Sun Pharmaceuticals, Constellation Software.

“Ransomware groups like BlackCat that are able to shift their tooling and tradecraft to make their operations faster and stealthier have a better chance of extending their lifespan,” IBM Security X-Force said in its post.

SC Media reported in May a Trend Micro finding that BlackCat was deploying a new kernel driver that leveraged a separate user client executable to control, pause and kill various processes on target endpoints of security agents deployed on protected computers. Updated tradecraft

That appears to be one of the attributes of a new version of its ransomware, which it calls Sphynx, that the group promoted to its affiliates in February. VX-Underground posted screenshots of an announcement on Twitter in which BlackCat said its ransomware “has been completely rewritten from scratch” and that the “main priority of this update was to optimize detection by AV/EDR (anti-virus/endpoint detection and response).”

“Sphynx differs from the previous variants in notable ways,” IBM Security X-Force’s analysis said.

“For example, the command line arguments have been reworked. Previous variants utilized the –access-token parameter in order to execute. The updated ransomware removes that parameter and adds a set of more complex arguments. This makes it harder to detect since defenders do not have standard commands to hunt.”

BlackCat switched to the Rust programming language in 2022, probably because it provided more opportunities to customize malware and hamper efforts to detect and analyze it, and the group’s affiliates continued to abuse the functionality of Group Policy Objects (GPO), both to deploy tools and to interfere with security measures, the researchers said. Fast and furious

“Attackers displaying a nuanced understanding of Active Directory can abuse GPOs to great effect for swift mass malware deployment. For example, threat actors may attempt to increase the speed of their operations by changing default Group Policy refresh times, likely to shorten the window of time between changes taking effect and defenders being able to respond.”

BlackCat attacks generally involved deploying tools for both data encryption and theft because the group usually ran a double extortion scheme.

“X-Force observed attackers leveraging ExMatter, a .NET data exfiltration tool that was introduced in 2021 and received a substantial update in August 2022. ExMatter is exclusively used by one BlackCat ransomware affiliate cluster, tracked by Microsoft as DEV-0504,” the researchers said.

“IBM X-Force has observed evidence that multiple terabytes of data had been exfiltrated from a victim environment to threat actor-controlled infrastructure. Stolen data is frequently posted publicly on the group’s official leak site in an attempt to apply pressure on extortion victims.”

In line with the more sophisticated tools other ransomware groups are deploying, X-Force said it expected BlackCat to continue increasing the speed and stealth of it operations using “novel means to accomplish different stages of their attacks”.

“Continuous advancements in BlackCat ransomware associated tradecraft, as well as the design of BlackCat and ExMatter malware, underscore adversary understanding of target systems and defender processes — as well as potential points where these can be leveraged for attacker advantage,” researchers wrote.



[https://www.scmagazine.com/news/ransomware/blackcat-ransomware-stealth-speed](https://www.scmagazine.com/news/ransomware/blackcat-ransomware-stealth-speed)
