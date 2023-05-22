---
description: '2023-05-22'
---

# BlackCat Ransomware Deploys New Signed Kernel Driver

## Executive Summary

In late December 2022, [Mandiant](https://www.mandiant.com/resources/blog/hunting-attestation-signed-malware), [Sophos](https://news.sophos.com/en-us/2022/12/13/signed-driver-malware-moves-up-the-software-trust-chain/) and [Sentinel One](https://www.sentinelone.com/labs/driving-through-defenses-targeted-attacks-leverage-signed-malicious-microsoft-drivers/), via a coordinated disclosure, reported malicious kernel drivers being signed through several Microsoft hardware developer accounts (certified by Microsoft’s Windows Hardware Developer Program). These profiles had been used in a number of cyberattacks that included [ransomware](https://www.trendmicro.com/vinfo/ph/security/definition/Ransomware)-based incidents. [Microsoft](https://msrc.microsoft.com/update-guide/vulnerability/ADV220005) subsequently revoked several Microsoft hardware developer accounts that were abused in these attacks.

In this blog post, we will provide details on a [BlackCat ransomware](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-blackcat/) incident that occurred in February 2023, where we observed a new capability, mainly used for the defense evasion phase, that overlaps with the earlier malicious drivers disclosed by the three vendors. [BlackCat](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-blackcat) affiliates have been known to use multiple techniques during the defense evasion phase, impairing defenses by disabling and modifying tools or using techniques as safe mode boot.

Our analysis sheds light on this new capability, which involves the use of a signed kernel driver for evasion. We believe that this new kernel driver is an updated version that inherited the main functionality from the samples disclosed in previous research. The driver was used with a separate user client executable in an attempt to control, pause, and kill various processes on the target endpoints related to the security agents deployed on the protected machines.

Malicious actors use different approaches to sign their malicious kernel drivers: Typically by abusing Microsoft signing portals, using leaked and stolen certificates, or using underground services. In our case, the attackers tried to deploy the old driver disclosed by Mandiant, which is signed through Microsoft (SHA256: b2f955b3e6107f831ebe67997f8586d4fe9f3e98). Since this driver has already been previously known and detected, the malicious actors deployed another kernel driver signed by a stolen or leaked cross-signing certificate. Trend Micro continues to monitor the abuse of any signed drivers and the related tools, tactics, and procedures (TTPs) associated with this attack surface.

## The malicious signed kernel drivers

The February 2023 ransomware incident we observed proves that ransomware operators and their affiliates have a high level of interest in gaining privileged-level access for the ransomware payloads they use in their attacks. They normally use ransomware families that incorporate low-level components to avoid detection from security products once the final payloads are dropped. By mapping the kill chains of these kernel-level threats, we found that [most kernel-related payloads are usually found during the defense evasion phase](https://www.trendmicro.com/vinfo/us/security/news/cybercrime-and-digital-threats/the-evolution-of-windows-kernel-threats), as shown in Figure 1.&#x20;





Some ransomware attacks try to comply with Microsoft code-signing requirements. This gives malicious actors the flexibility to compile kernel modules designed for very specific tasks (usually involving defense impairing and evasion) before dropping the actual payload. Ransomware operators can do one of the following approaches:

1\. Use a code-signing certificate that was either leaked, stolen from a compromised environment, or purchased from an underground market.

2\. Obtain a new valid code-signing certificate by impersonating a legitimate entity and following Microsoft’s process for getting the cross-signing certificate (back when Microsoft still allowed cross signing for kernel-mode code), abusing Microsoft’s portal for issuing signed kernel modules, and purchasing valid code-signing certificates and/or Extended Validation (EV) certificates that are tied to real identities from underground markets.



## Analysis of a signed driver

In this section, we will examine a signed driver (_ktgn.sys_) used in the February BlackCat attacks. Figure 4 shows other examples of these new signed drivers and how they are being used as part of the BlackCat affiliate’s defense evasion routine.



The User Agent _tjr.exe_, which is protected via a virtual machine, drops the kernel driver to the user temporary directory _C:\\%User%\AppData\Local\Temp\Ktgn.sys_. It then installs the dropped driver with the name _ktgn_ and the start value = System (to start when the system restarts). From our analysis of what occurs when a user interfaces with this driver, we observed that it only uses one of the exposed Device Input and Output Control (IOCTL) code — Kill Process, which is used to kill security agent processes installed on the system.

Meanwhile the driver _ktgn.sys_, which is signed using a currently revoked valid digital signature from “BopSoft” (which had also been previously used by other threat actors for code signing) can successfully be loaded on a 64-bit Windows installation where signing policies are enforced. The driver is obfuscated using [_Safengine Protector v2.4.0.0_](http://www.safengine.com/en-us/products/protector) tool, which renders static analysis techniques unreliable. By loading the obfuscated driver and trying to build a user mode client to observe the exposed IOCTL interface, we can determine the function of each IOCTL code. Finally, we observed the same kernel driver being signed by different code-signing certificates.

| Driver variants (SHA256)                 | Signer name | Valid usage  | Current status                   | Issuer   |
| ---------------------------------------- | ----------- | ------------ | -------------------------------- | -------- |
| 994e3f5dd082f5d82f9cc84108a60d359910ba79 | BopSoft     | Code signing | Explicitly revoked by its issuer | Thawte   |
| f6793243ad20359d8be40d3accac168a15a327fb | YI ZENG     | Code signing | Explicitly revoked by its issuer | VeriSign |

**Table 1. The driver variants with different signers**



Since it does not register an unload callback function, the driver can only be unloaded if the service registry key is deleted or modified followed by a system restart.





A symbolic link with the name _\\\\.\keHeperDriverLink_ is created that allows the user mode client to connect and communicate with it. Note that this link only allows for one connection — if more than one client tries to connect to it simultaneously, the system will crash.



## The exposed IOCTL Interface

This client supports ten different commands, with each command implementing a specific function that is executed from the kernel driver with the appropriate IOCTL interface exposed. Communication between the driver and the user mode client occurs using the IRP\_MJ\_DEVICIDE\_CONROL handler via the following codes:

| IOCTL Code | Description                                   |
| ---------- | --------------------------------------------- |
| 222088h    | Activate Driver                               |
| 22208Ch    | Deactivate Driver                             |
| 222094h    | Kill Process                                  |
| 222184h    | Delete File                                   |
| 222188h    | Force Delete File                             |
| 22218Ch    | Copy File                                     |
| 222190h    | Force Copy File                               |
| 2221C8h    | Register Process/Thread Object notification   |
| 2221C4h    | Unregister Process/Thread Object notification |
| 222264h    | Reboot the system                             |

**Table 2. Each IOCTL code and their function**

Based on our analysis of the kernel driver, it seems to still be under development and testing since it is not structured well and some of its functions currently cannot be used. The following subsections provide details on the various IOCTL Interfaces.

#### IOCTL 222088h

IOCTL 222088h must be called first to activate the driver before any other operation can be performed. If this code is not called, the driver will not accept any operation and will return the message _STATUS\_ACCESS\_DENIED_. The user mode client sends this activation byte array to the driver.

The activation is a simple byte comparison against a hard coded byte array with the size 0x42 located in the driver. If the comparison passes, it will set a BOOLEAN flag, which will be checked before any operation.





#### IOCTL 22208Ch

IOCTL 22208Ch is called after the user mode client finishes its operation to unset the flag that was previously set in IOCTL Code 222088h. This will deactivate the driver and stop it from processing any new operation.

The client will need to pass the same byte array passed in IOCTL code 222088h for the operation to be successfully completed.

#### IOCTL 222094h

IOCTL 222094h is used to kill any user mode process (even protected ones). Tt receives the Process ID from the user agent then creates a kernel thread in the target process context. The created kernel thread calls the ZwTerminateProcess API to terminate the target process.&#x20;





#### IOCTL 222184h

IOCTL 222184h is used to delete specific file paths (as shown in Figure 11).



IOCTL 222188h\


IOCTL 222188h is used to force delete files. To do this, the kernel driver does the following:

1. It tries to open all processes on the system using brute-force methods (starting from PID=0x4 to PID= 0x27FFD)
2. When it successfully opens a process, it tries to reference all handles inside the process, again using a brute-force method (starting from HANDLE=0x4 to HANDLE = 0x27FFD)
3. When it successfully references a handle, it uses the ObQueryNameString API to map the handle to a name. When a match is found, the kernel driver closes the handle.

This operation will ensure that all references to the file will be closed and the operation can be successfully completed without any errors stating that the file is being used by other applications.





#### IOCTL 22218Ch

IOCTL 22218Ch is used to copy files.&#x20;



#### IOCTL 222190h

IOCTL 222190h is used to force copy files. The driver uses the same operation as the one used for force deletion (IOCTL Code: 222188h). It closes all references to the files from all processes using brute-force methods, then copies the file.

#### IOCTL 2221C4h and IOCTL 2221C8h

Both IOCTL 2221C4h and 2221C8h are used to register and unregister Process/Thread Notification callbacks. However, both paths are unreachable at the time of writing, which indicates that they are still under development or testing.







#### IOCTL 222264h

IOCTL 222264h Is used to reboot the system by calling the HalReturnToFirmware API.

## Conclusion

Malicious actors that are actively seeking high-privilege access to the Windows operating system use techniques that attempt to combat the increased protection on users and processes via endpoint protection platform (EPP) and endpoint detection and response (EDR) technologies. Because of these added layers of protection, attackers tend to opt for the path of least resistance to get their malicious code running via the kernel layer (or even lower levels). This is why we believe that such threats will not disappear from threat actors’ toolkits anytime soon.

Malicious actors will continue to use rootkits to hide malicious code from security tools, impair defenses, and fly under the radar for long periods. These rootkits will see heavy use from sophisticated groups that have both the skills to reverse engineer low-level system components and the required resources to develop such tools. These malicious actors also tend to possess enough financial resources to either purchase rootkits from underground sources or to buy code-signing certificates to build a rootkit. This means that the main danger involving these kinds of rootkits lie in their ability to hide complex targeted attacks that will be used early in the kill chain, allowing an attacker to impair defenses before the actual payloads are launched in victim environments.

## Recommendations and solutions

Code signing certificates can often be abused by threat actors since they provide an additional layer of obfuscation in their attacks. For organizations, compromised keys present not only a security risk, but can also lead to a loss of reputation and trust in the original signed software. Businesses should aim to protect their certificates by implementing best practices such as reducing access to private keys, which reduces the risk of unauthorized access to the certificate. Employing strong passwords and other authentication methods for private keys can also help protect them from being stolen or compromised by malicious actors. Furthermore, using separate test signing certificates (for prerelease code used in test environments) minimizes the chances that the actual release signing certificates are abused in an attack.

For general ransomware attack protection, organizations can implement a systematic security framework that allocates resources towards establishing a robust defense strategy. Here are some recommended guidelines:

* Take inventory of assets and data
* Identify authorized and unauthorized devices and software
* Audit event and incident logs
* Manage hardware and software configurations
* Grant admin privileges and access only when necessary
* Monitor network ports, protocols, and services
* Establish a software allowlist for legitimate applications
* Implement data protection, backup, and recovery measures
* Enable multifactor authentication (MFA)
* Deploy the latest versions of security solutions across all layers of the system
* Watch for early signs of an attack

By adopting a multifaceted approach to securing potential entry points, such as endpoints, emails, webs, and networks, organizations can detect and protect against malicious elements and suspicious activities, thereby safeguarding themselves from ransomware attacks.

A multilayered approach can help organizations guard possible entry points into their system (endpoint, email, web, and network). Security solutions can detect malicious components and suspicious behavior, which can help protect enterprises. \


## Indicators of Compromise

The indicators of compromise for this entry can be found [here](https://www.trendmicro.com/content/dam/trendmicro/global/en/research/23/e/blackcat-ransomware-deploys-new-signed-kernel-driver/indicators-blackcat-ransomware-deploys-new-signed-kernel-driver.txt).



[https://www.trendmicro.com/en\_us/research/23/e/blackcat-ransomware-deploys-new-signed-kernel-driver.html](https://www.trendmicro.com/en\_us/research/23/e/blackcat-ransomware-deploys-new-signed-kernel-driver.html)
