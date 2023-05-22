---
description: '2023-05-22'
---

# Malicious Windows kernel drivers used in BlackCat ransomware attacks

The ALPHV ransomware group (aka BlackCat) was observed employing signed malicious Windows kernel drivers to evade detection by security software during attacks.

The driver seen by Trend Micro is an improved version of the malware known as 'POORTRY' that Microsoft, Mandiant, Sophos, and SentinelOne spotted in ransomware attacks [late last year](https://www.bleepingcomputer.com/news/microsoft/microsoft-signed-malicious-windows-drivers-used-in-ransomware-attacks/).&#x20;

The POORTRY malware is a Windows kernel driver signed using stolen keys belonging to legitimate accounts in Microsoft's Windows Hardware Developer Program.

This malicious driver was used by the UNC3944 hacking group, also known as 0ktapus and Scattered Spider, to terminate security software running on a Windows device to evade detection.

While security software is usually protected from being terminated or tampered with, as Windows kernel drivers run with the highest privileges in the operating system, they can be used to terminate almost any process.

Trend Micro says the ransomware actors attempted to use the Microsoft-signed POORTRY driver, but its detection rates were high following the publicity it got and after the code-signing keys were revoked.

Hence, the hackers deployed an updated version of the POORTRY kernel driver signed using a stolen or leaked cross-signing certificate.

The new driver used by the [BlackCat ransomware operation](https://www.bleepingcomputer.com/news/security/alphv-blackcat-this-years-most-sophisticated-ransomware/) helps them elevate their privileges on compromised machines and then stop processes relating to security agents.&#x20;

Furthermore, it may provide a loose link between the ransomware gang and the UNC3944/Scattered Spider hacking groups.

### The Malicious Windows kernel driver

The signed driver seen by Trend Micro in February 2023 BlackCat attacks is 'ktgn.sys,' dropped onto the victim's filesystem in the %Temp% folder and then loaded by a user mode program named 'tjr.exe.'

The analysts say the digital signature of ktgn.sys has been revoked; however, the driver will still load without a problem on 64-bit Windows systems with enforced signing policies.

The malicious kernel driver exposes an IOCTL interface that allows the user mode client, tjr.exe, to issue commands that the driver will execute with Windows kernel privileges.

"From our analysis of what occurs when a user interfaces with this driver, we observed that it only uses one of the exposed Device Input and Output Control (IOCTL) code — Kill Process, which is used to kill security agent processes installed on the system," explains the [Trend Micro report](http://www.trendmicro.com/en\_us/research/23/e/blackcat-ransomware-deploys-new-signed-kernel-driver.html).

<figure><img src="https://www.bleepstatic.com/images/news/u/1220909/2023/Ransomware/27/diagram.png" alt="Malicious drivers used in BlackCat attacks" height="600" width="551"><figcaption><p><strong>Malicious drivers used in BlackCat attacks</strong> <em>(Trend Micro)</em></p></figcaption></figure>

Trend Micro's analysts observed the exposed following commands that can be issued to the driver:

1. Activate driver
2. Deactivate the driver after the user mode client finishes its operation
3. Kill any user-mode process
4. Delete specific file paths
5. Force-delete a file by freeing its handles and terminating running processes using it
6. Copy files
7. Force-copy files using a similar mechanism to force-delete
8. Register Process/Thread Notification callbacks
9. Unregister Process/Thread Notification callbacks
10. Reboot the system by calling the 'HalReturnToFirmware' API

<figure><img src="https://www.bleepstatic.com/images/news/u/1220909/2023/Ransomware/27/copy-file.png" alt="Copying files from the system" height="142" width="893"><figcaption><p><strong>Copying files from the system</strong> <em>(Trend Micro)</em></p></figcaption></figure>

Trend Micro comments that the two commands used for Process/Thread Notification callbacks are not working, indicating that the driver is currently under development or still in a testing phase.

System administrators are recommended to use the indicators of compromise shared by Trend Micro and add the malicious drivers used by the ransomware actors to the [Windows driver blocklist](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/microsoft-recommended-driver-block-rules).

Windows admins should also ensure that 'Driver Signature Enforcement' is enabled, which blocks the installation of any drivers that do not have a valid digital signature.
