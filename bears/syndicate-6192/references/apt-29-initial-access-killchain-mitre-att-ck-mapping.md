---
description: '2023-05-24'
---

# APT 29 Initial Access Killchain -MITRE ATT@CK Mapping

### **APT29/Nobelium Initial Access & ATT@CK Mapping** <a href="#934f" id="934f"></a>

**TA0042: Resource Development**

* T1650: Aquire Infrastructure
* T1584: Compromised Infrastructure
* T1587: Develop Capabilities
* T1587.001: Develop Capabilities: Malware
* T1588: Obtain Capabilities
* T1588.002: Obtain Capabilities: Tool

APT29/Nobelium Cobalt Strike C2 setup with custom certificates and redirections (Pay attention to how similar threat actor communitypowersports\[.]com domain is to the genuine sanjosemotosport\[.]com).

These domain similarities or sometimes typosquatting SSL domains are techniques used frequently by Threat Actors.

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*7qeYJtPaxYrY1HufxVoNpg.png" alt="" height="277" width="1000"><figcaption><p>APT29/Nobelium Cobalt Strike C2 redirector setup</p></figcaption></figure>

Here you can see how the mode rewrite redirector works.

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*Zui2pObKxN6uZh1YwreTDA.png" alt="" height="292" width="1000"><figcaption><p>Cobalt Strike C2 mode rewrite setup</p></figcaption></figure>

**Initial Access Attack Analysis HTML (EnvyScout) dropper used by Russian APT29/Nobelium in recent campaigns.**

EnvyScout uses a technique known as HTML smuggling to deliver an IMG/ISO file to the targeted systems (data block that can be decoded by subtracting 4 in the recent campaign).

After decoding you will find an ISO file inside that contains SnowyAmber that executes via rundll32.exe and communicate to Notion API as a C2.

**Initial Access**

* T1566: Phishing
* T1566.001: Spearphishing Attachment
* T1566.002: Spearphishing Link

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*_15wVB-rCFJQvSs1wM6xjQ.png" alt="" height="571" width="700"><figcaption><p>ISO File containing SnowyAmber</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*DG_yM0KzIP9-OyyHEEHD6g.png" alt="" height="393" width="1000"><figcaption><p>Compromised website</p></figcaption></figure>

**Execution**

* T1204: User Execution
* T1204.001: Malicious Link
* T1204.002: Malicious File

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*t8EqLHM7mh2KkW6ovBr4Gw.png" alt="" height="582" width="1000"><figcaption><p>APT29 EnvyScout and Snowy Amber</p></figcaption></figure>

**Defence Evasion**

* T1027.006: HTML Smuggling
* T1036.002: Right-to-Left Override
* T1140: Deobfuscate/Decode Files or Information
* T1218.011: System Binary Proxy Execution: Rundll32
* T1553.005: Mark-of-the-Web Bypass

**Persistence**

* T1547.001: Registry Run Keys / Startup Folder
* T1574.001: DLL Search Order Hijacking
* T1574.002: DLL Side-Loading

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*UTKIv941SSu_exGDYqzpew.png" alt="" height="321" width="1000"><figcaption><p>SnowyAmber execution via Rundll32</p></figcaption></figure>

**C2**

* T1102: Web Service
* T1102.003: One-Way Communication

<figure><img src="https://miro.medium.com/v2/resize:fit:1000/1*TrKwtKWV17LYTDVKbvEKEg.png" alt="" height="335" width="1000"><figcaption><p>APT29/Nobelium C2 Infrastructure</p></figcaption></figure>

**APT29/Nobelium Initial Access Killchain**

<figure><img src="https://miro.medium.com/v2/resize:fit:4216/1*ejZweFCwVTjTKSwu_Ss1qA.png" alt="" height="2040" width="2400"><figcaption><p>APT29/Nobelium Initial Access Killchain</p></figcaption></figure>

Ref:

[Espionage campaign linked to Russian intelligence services - Baza wiedzy - Portal Gov.plThe Military Counterintelligence Service and the CERT Polska team (CERT.PL) observed a widespread espionage campaign…www.gov.pl](https://www.gov.pl/web/baza-wiedzy/espionage-campaign-linked-to-russian-intelligence-services)



[https://michaelkoczwara.medium.com/apt-29-initial-access-killchain-mitre-att-ck-mapping-f82286fa13ba](https://michaelkoczwara.medium.com/apt-29-initial-access-killchain-mitre-att-ck-mapping-f82286fa13ba)
