# Bitter APT Bangladesh

## Description

Since August 2021. This campaign is a typical example of the actor targeting South Asian government entities.

This campaign targets an elite unit of the Bangladesh's government with a themed lure document alleging to relate to the regular operational tasks in the victim's organization. The lure document is a spear-phishing email sent to high-ranking officers of the Rapid Action Battalion Unit of the Bangladesh police (RAB). The emails contain either a malicious RTF document or a Microsoft Excel spreadsheet weaponized to exploit known vulnerabilities. Once the victim opens the maldoc, the Equation Editor application is automatically launched to run the embedded objects containing the shellcode to exploit known vulnerabilities described by CVE-2017-11882, CVE-2018-0798 and CVE-2018-0802 — all in Microsoft Office — then downloads the trojan from the hosting server and runs it on the victim's machine. The trojan masquerades as a Windows Security update service and allows the malicious actor to perform remote code execution, opening the door to other activities by installing other tools. In this campaign, the trojan runs itself but the actor has other RATs and downloaders in their arsenal.

Such surveillance campaigns could allow the threat actors to access the organization's confidential information and give their handlers an advantage over their competitors, regardless of whether they're state-sponsored.
