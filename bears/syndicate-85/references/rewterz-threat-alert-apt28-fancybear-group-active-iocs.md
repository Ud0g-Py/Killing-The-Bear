---
description: '2023-05-09'
---

# Rewterz Threat Alert – APT28 FancyBear Group – Active IOCs

### Severity

High

### Analysis Summary

APT28 is one of Russia’s longest-running APTs and its operations date back to at least 2007. The group supports Russia in their strategic operations against the U.S, countries of the former Soviet Union, Europe, and now Asia. These attacks mostly involve cyber crimes against the defense and military of targeted countries. To support Russia’s national interests, APT28 compromises the targeted country’s operation, steals their data, and then leaks it to their government.

&#x20;Going by the aliases**Fancy Bear**

, Pawn Storm, Tsar Team, STRONTIUM, and Sofacy Group, APT28 performs their attacks using a spoofed website and phishing emails containing malicious links.&#x20;

In Feb 2022, APT 28 (allegedly) attacked Eastern European countries using Empire and Invoke-Obfuscation. The MSHTML Remote Code Execution vulnerability,

**CVE-2021-40444**

, that was used by their threat actors

<figure><img src="https://app.sirp.io/uploads/1/image-editor/threat-intelligence/image-editor-1683097871.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://app.sirp.io/uploads/1/image-editor/threat-intelligence/image-editor-1683097890.png" alt=""><figcaption></figcaption></figure>

The recent campaign include malicious files named, “1e88179a-3105-4a5c-9eb3-aebea36e9c21.ps1” and “niso55.ps1,” which appear to be PowerShell script filenames. PowerShell is a powerful scripting language and automation framework developed by Microsoft, commonly used in Windows environments.

The use of PowerShell scripts in cyberattacks is not uncommon. Malicious actors often employ PowerShell to execute various activities, including reconnaissance, lateral movement, and data exfiltration.

By using PowerShell, attackers can leverage the functionality and privileges provided by the Windows operating system to perform their malicious actions.

### Impact

* Information Theft
* Data Exfiltration
* Exposure of Sensitive Data

### Indicators of Compromise

**MD5**

* d5ab587aaa4bf24d17ab42179b798b10
* 66ee3445486859eee2d36028a1a64bb9

**SHA-256**

* e6d3217f89dc53f97989f05188b19f090dbbe1510a17c31398bcfeafa2fe7cba
* f1b937bdd6c3fac6dfde33bec229c378bdce92b4e736afec4084c98a899ef295

**SHA-1**

* b27311413076be38dd8a115061edda9cd0ba51b3
* cd3dc8f564131f20401c97c1feba7c452b7691e7

**URL**

https://run.mocky.io/v3/1e88179a-3105-4a5c-9eb3-aebea36e9c21/

http://mockbin.org/bin/e8bfd045-2b14-4afc-9372-b723f7d76918

### Remediation

* Block all threat indicators at your respective controls.
* Search for Indicator of compromise (IOCs) in your environment utilizing your respective security controls
* Ensure that general security policies are employed including: implementing strong passwords, correct configurations, and proper administration security policies.
* Emails from unknown senders should always be treated with caution.
* Never open links or attachments from unknown senders.
