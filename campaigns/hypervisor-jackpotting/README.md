---
cover: ../../.gitbook/assets/hypervisor-blog-1.jpg
coverY: 0
---

# Hypervisor Jackpotting

## Description

#### By Anomali

Recent reporting indicates that two prolific cybercrime threat groups, CARBON SPIDER and SPRITE SPIDER, have begun targeting ESXi, a hypervisor developed by VMWare to run and manage virtual machines. SPRITE SPIDER uses PyXie's LaZagne module to recover vCenter credentials stored in web browsers and runs Mimikatz to steal credentials from host memory. After authenticating to vCenter, SPRITE SPIDER enables ssh to permit persistent access to ESXi devices. In some cases, they also change the root account password or the host’s ssh keys. Before deploying Defray 777, SPRITE SPIDER’s ransomware of choice, they terminate running VMs to allow the ransomware to encrypt files associated with those VMs. CARBON SPIDER has traditionally targeted companies operating POS devices, with initial access being gained using low-volume phishing campaigns against this sector. But throughout 2020 they were observed shifting focus to “Big Game Hunting” with the introduction of the Darkside Ransomware. CARBON SPIDER gains access to ESXi servers using valid credentials and reportedly also logs in over ssh using the Plink utility to drop the Darkside

{% hint style="danger" %}
Tags:\
PyXie, Cobalt Strike, REvil, Vatet, LaZagne, Sekur, PowerSploit, Mimikatz, Griffon, Darkside, Anunak, Target777, BokBot, Defray, RansomEXX, Vatet, Defray777, CARBON SPIDER, Group 24, Defray 2018, PINCHY SPIDER, LUNAR SPIDER, Target777, SPRITE SPIDER
{% endhint %}

## 2021

### Aug

#### [Hypervisor Jackpotting, Part 2: eCrime Actors Increase Targeting of ESXi Servers with Ransomware](https://www.evernote.com/shard/s724/sh/ac2834c9-162a-4e17-8d85-ddfec7bcf371/91c8164dec7b1c0324b326c4436f5a68)

### Feb

#### [Hypervisor Jackpotting, Part 1: CARBON SPIDER and SPRITE SPIDER Target ESXi Servers With Ransomware to Maximize Impact](https://www.evernote.com/shard/s724/sh/af3b1473-d9da-4489-bf3c-ca1e64044459/38e4002bcb467414684533d518cad336)
