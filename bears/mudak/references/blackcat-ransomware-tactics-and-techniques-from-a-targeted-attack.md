---
description: '2022-11-09'
---

# BlackCat Ransomware: Tactics and Techniques From a Targeted Attack

### Summary <a href="#h-summary" id="h-summary"></a>



[**BlackCat**](https://id-ransomware.blogspot.com/2021/12/blackcat-ransomware.html)(a.k.a.**ALPHV**and**Noberus**) is a Ransomware-as-a-Service (RaaS) group that emerged in [November 2021](https://www.bleepingcomputer.com/news/security/alphv-blackcat-this-years-most-sophisticated-ransomware/), making headlines for being a sophisticated ransomware written in Rust. It has both [Windows](https://malpedia.caad.fkie.fraunhofer.de/details/win.blackcat) and [Linux](https://malpedia.caad.fkie.fraunhofer.de/details/elf.blackcat) variants and the payload can be customized to adapt to the attacker’s needs. BlackCat is also [believed](https://symantec-enterprise-blogs.security.com/blogs/threat-intelligence/noberus-blackcat-ransomware-ttps) to be the successor of the [Darkside](https://www.netskope.com/pt/blog/netskope-threat-coverage-darkside) and[**BlackMatter**](https://www.netskope.com/blog/netskope-threat-coverage-blackmatter)

ransomware groups. They work with a double-extortion scheme, where data is stolen, encrypted, and leaked if the ransom isn’t paid, which is a common methodology implemented by RaaS groups.&#x20;

According to Microsoft, BlackCat [was found](https://www.microsoft.com/security/blog/2022/06/13/the-many-lives-of-blackcat-ransomware/) targeting different countries and regions in Africa, the Americas, Asia, and Europe, having at least two known affiliates: [DEV-0237](https://www.microsoft.com/security/blog/2022/05/09/ransomware-as-a-service-understanding-the-cybercrime-gig-economy-and-how-to-protect-yourself/#DEV-0237) (previously associated with **Ryuk**,**Conti**, and**Hive**), and [DEV-0504](https://www.microsoft.com/security/blog/2022/05/09/ransomware-as-a-service-understanding-the-cybercrime-gig-economy-and-how-to-protect-yourself/#DEV-0504) (previously associated with Ryuk,**REvil**

, BlackMatter, and Conti). However, due to the diversity of affiliates and targets, BlackCat may present different TTPs across the attacks. Recently, in September 2022, BlackCat [claimed](https://www.theregister.com/2022/10/02/in-brief-security/) to have breached a contractor that provides services to the U.S. Department of Defense and other government agencies.&#x20;

In this blog post, we will analyze BlackCat and show some of the tactics and techniques we found in a recent ransomware incident analyzed by Netskope Threat Labs. The evidence shows that this was a targeted attack, where the attackers were mainly focused on stealing sensitive data from the organization and infecting as many devices as possible.

In a recent incident analyzed by Netskope Threat Labs, the attackers breached a contractor who had

**access**

to a virtual desktop machine within the corporate network.

The attacker used a malicious browser extension to capture the contractor’s account. Since there was no MFA required, the attacker was able to login to the virtual desktop, escalate privileges, and move to other devices in the corporate network.

### Payload Execution <a href="#h-payload-execution" id="h-payload-execution"></a>

After scanning the corporate network, BlackCat attackers created multiple

**text**

files, each one containing the names of identified machines in the network.

Then, they used [PsExec](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec) and a compromised domain account to deploy

**ExMatter**

to more than 2,000 machines in the network.

The attackers used batch files to execute multiple PsExec commands to deploy payloads to the identified machines.

Below is an example of the command line executed by the attacker to remotely execute commands and payloads using PsExec and the compromised account:

start PsExec.exe -d -n 5 @C:\temp\list01.txt -accepteula -u \<REDACTED\_USER> -p \<REDACTED\_PASSWORD> cmd /c \<COMMAND\_LINE>

The description for the PsExec arguments used by the attacker can be found below:

| Argument | Description                                  |
| -------- | -------------------------------------------- |
| -d       | Don't wait for process to terminate (**non** |

| -interactive)       |                                                                                     |
| ------------------- | ----------------------------------------------------------------------------------- |
| -n 5                | Wait 5 seconds when connecting to remote computers                                  |
| @C:\temp\list01.txt | File containing the names of the computers in which PsExec will execute the command |
| -accepteula         | Automatically accept the EULA to avoid displaying the dialog                        |
| -u                  | Username of the compromised account used by the attacker                            |
| -p                  | Password of the compromised account used by the attacker                            |
| cmd /c              | Command-line executed by the attacker                                               |

Among other evidence, it’s possible to confirm whether PsExec was successfully executed in a device by checking the following registry key.

### Data Exfiltration

In this incident, the attackers used a .NET data exfiltration tool known as [ExMatter](https://malpedia.caad.fkie.fraunhofer.de/details/win.exmatter), which was the same tool used by BlackMatter ransomware and [recently adopted](https://www.bleepingcomputer.com/news/security/blackcat-ransomware-s-data-exfiltration-tool-gets-an-upgrade/) by BlackCat. It’s worth mentioning that the server used for data exfiltration in this incident was stood up by the attackers one day before the attack.

The specific sample from this incident was compiled close to the attack and contains a popular .NET protection named [Confuser](https://github.com/mkaring/ConfuserEx).

The attacker tried to deploy this tool to over 2,000 machines in the network using PsExec, like described earlier. ExMatter will iterate over the drives of infected machines to search for files that will be exfiltrated.

As described earlier, this tool was [recently updated](https://www.bleepingcomputer.com/news/security/blackcat-ransomware-s-data-exfiltration-tool-gets-an-upgrade/) by BlackCat, containing code refactoring and new functionalities. Despite the code changes, we can clearly observe similarities between a [known ExMatter](https://www.virustotal.com/gui/file/8eded48c166f50be5ac33be4b010b09f911ffc155a3ab76821e4febd369d17ef) sample and the tool used in this attack.

ExMatter contains a list with details about the types of files it will try to exfiltrate and directories to avoid. Also, this tool is only stealing files between **4 KB** and **64 MB**.

It will not exfiltrate data from the following directories:

* AppData\Local\Microsoft
* AppData\Local\Packages
* AppData\Roaming\Microsoft
* C:$Recycle.Bin
* C:\Documents and Settings
* C:\PerfLogs
* C:\Program Files
* C:\Program Files (x86)
* C:\ProgramData
* C:\Users\All Users\Microsoft
* C:\Windows

As previously mentioned, it will only exfiltrate files that contains the following extensions and are within the file size threshold:

* \*.bmp
* \*.doc
* \*.docx
* \*.dwg
* \*.ipt
* \*.jpeg
* \*.jpg
* \*.msg
* \*.pdf
* \*.png
* \*.pst
* \*.rdp
* \*.rtf
* \*.sql
* \*.txt
* \*.txt
* \*.xls
* \*.xlsx
* \*.zip

By default, this specific sample is trying to communicate with an IP address via [WebDav](http://www.webdav.org/), initially sending a [PROPFIND](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142960\(v=exchg.65\)) request.

The WebDav methods implemented by this tool are: [PROPFIND](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142960\(v=exchg.65\)), [PROPPATCH](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142976\(v=exchg.65\)), [MKCOL](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142923\(v=exchg.65\)), [COPY](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142816\(v=exchg.65\)), [MOVE](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142926\(v=exchg.65\)), [LOCK](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa142897\(v=exchg.65\)), and [UNLOCK](https://learn.microsoft.com/en-us/previous-versions/office/developer/exchange-server-2003/aa143146\(v=exchg.65\)).

This tool can also be executed in background (without showing the console) if “**-background**” or “**-b**” is specified.

### Data Encryption

Like the ExMatter tool, the BlackCat payload was also compiled in July 2022. The attackers deployed the ransomware to over 2,000 machines with the same technique described earlier, by using PsExec with a compromised domain account.

BlackCat can be executed with different parameters, which can be found in its “help” menu.

The options offered by BlackCat ransomware are:

| Parameter                   | Description                                                                                                                                                                                    |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --access-token              | String used by BlackCat to validate the execution. It’s also used to decrypt BlackCat configuration in the latest version                                                                      |
| --bypass                    | This parameter doesn’t seem to be implemented                                                                                                                                                  |
| --child                     | Run as child process                                                                                                                                                                           |
| --drag-and-drop             | Invoked with drag and drop                                                                                                                                                                     |
| --drop-drag-and-drop-target | Drop drag and drop target batch file                                                                                                                                                           |
| --extra-verbose             | Log more to console (Also forces process to run in attached mode)                                                                                                                              |
| -h, --help                  | Print help information                                                                                                                                                                         |
| --log-file                  | Enable logging to specified file                                                                                                                                                               |
| --no-impers                 | Do not spawn impersonated processes on Windows                                                                                                                                                 |
| --no-net                    | Do not discover network shares on Windows                                                                                                                                                      |
| --no-prop                   | Do not self propagate (worm) on Windows                                                                                                                                                        |
| --no-prop-servers           | Do not propagate to defined servers                                                                                                                                                            |
| --no-vm-kill                | Do not stop VMs on ESXi                                                                                                                                                                        |
| --no-vm-kill-names          | Do not stop defined VMs on ESXi                                                                                                                                                                |
| --no-vm-snapshot-kill       | Do not wipe VMs snapshots on ESXi                                                                                                                                                              |
| --no-wall                   | Do not update desktop wallpaper on Windows                                                                                                                                                     |
| -p, --paths                 | Only process files inside defined paths                                                                                                                                                        |
| --prop-file                 | Propagate specified file                                                                                                                                                                       |
| --propagated                | Run as propagated process                                                                                                                                                                      |
| --safeboot                  | Reboot in Safe Mode before running on Windows                                                                                                                                                  |
| --safeboot-instance         | Run as safeboot instance on Windows                                                                                                                                                            |
| --safeboot-network          | Reboot in Safe Mode with Networking before running on Windows                                                                                                                                  |
| --sleep-restart             | Sleep for duration in seconds after a successful run and then restart. (This is soft persistence, keeps process alive no longer then defined in --sleep-restart-duration, 24 hours by default) |
| --sleep-restart-duration    | Keep soft persistence alive for duration in seconds. (24 hours by default)                                                                                                                     |
| --sleep-restart-until       | Keep soft persistence alive until defined UTC time in millis. (Defaults to 24 hours since launch)                                                                                              |
| --ui                        | Show user interface                                                                                                                                                                            |
| -v, --verbose               | Log to console                                                                                                                                                                                 |

At this point, two versions of BlackCat’s encryptor were found in the wild. The first one was storing the ransomware’s configuration in plain-text within the binary, which could be easily [extracted and parsed](https://github.com/f0wl/blackCatConf). The second one started to [encrypt the configuration](https://twitter.com/vxunderground/status/1504207503734804484), where the decryption key is passed via an argument named “access token”. In other words, the latest version of BlackCat cannot be executed or have its configuration extracted if the access token is unknown.&#x20;

The version used in this specific attack is the latest one, which can be confirmed by running the sample without the access key or with an random key, generating an “invalid config” error.

Once running, the access key is then parsed and used to decrypt the configuration in runtime, using AES-128.

BlackCat ransomware’s configuration contains 23 fields:

| Value                            | Description                                                                                                                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| config\_id                       | Configuration ID (used by BlackCat to identify the target)                                                                                                                                                               |
| extension                        | Extension added to encrypted files                                                                                                                                                                                       |
| public\_key                      | RSA public key                                                                                                                                                                                                           |
| note\_file\_name                 | Name of the ransom note                                                                                                                                                                                                  |
| note\_full\_text                 | Full version of the ransom note                                                                                                                                                                                          |
| note\_short\_text                | Short version of the ransom note                                                                                                                                                                                         |
| credentials                      | Array of compromised credentials used by BlackCat for privilege escalation and propagation via PsExec                                                                                                                    |
| default\_file\_mode              | File encryption mode, usually set as “Auto”. The “SmartPattern” value was also [found](https://www.varonis.com/blog/blackcat-ransomware) in the wild, which resulted in just some megabytes of the file being encrypted. |
| default\_file\_cipher            | File encryption cipher, usually defined as “Best”, which uses AES.                                                                                                                                                       |
| kill\_services                   | List of services to be terminated                                                                                                                                                                                        |
| kill\_processes                  | List of processes to be terminated                                                                                                                                                                                       |
| exclude\_directory\_names        | List of directories to exclude from the encryption process                                                                                                                                                               |
| exclude\_file\_names             | List of files to exclude from the encryption process                                                                                                                                                                     |
| exclude\_file\_extensions        | List of extensions to exclude from the encryption process                                                                                                                                                                |
| exclude\_file\_path\_wildcard    | File paths to be excluded from the encryption process using wildcard                                                                                                                                                     |
| enable\_network\_discovery       | Enable/disable network discovery                                                                                                                                                                                         |
| enable\_self\_propagation        | Enable/disable self propagation via PsExec                                                                                                                                                                               |
| enable\_set\_wallpaper           | Enable/disable the wallpaper change                                                                                                                                                                                      |
| enable\_esxi\_vm\_kill           | Enable/disable VM termination on ESXi                                                                                                                                                                                    |
| enable\_esxi\_vm\_snapshot\_kill | Enable/disable snapshot deletion on ESXi                                                                                                                                                                                 |
| strict\_include\_paths           | Hardcoded file paths to encrypt                                                                                                                                                                                          |
| esxi\_vm\_kill\_exclude          | List of VMs to exclude on ESXi hosts                                                                                                                                                                                     |
| sleep\_restart                   | Sleep time before restart                                                                                                                                                                                                |

According to the decrypted configuration of this specific sample, the ransomware tries to kill the following services:

* agntsvc
* dbeng50
* dbsnmp
* encsvc
* excel
* firefox
* infopath
* isqlplussvc
* msaccess
* mspub
* mydesktopqos
* mydesktopservice
* notepad
* ocautoupds
* ocomm
* ocssd
* onenote
* oracle
* outlook
* powerpnt
* sqbcoreservice
* sql
* steam
* synctime
* tbirdconfig
* thebat
* thunderbird
* visio
* winword
* wordpad
* xfssvccon
* \*sql\*
* bedbh
* vxmon
* benetns
* bengien
* pvlsvr
* beserver
* raw\_agent\_svc
* vsnapvss
* CagService
* QBIDPService
* QBDBMgrN
* QBCFMonitorService
* SAP
* TeamViewer\_Service
* TeamViewer
* tv\_w32
* tv\_x64
* CVMountd
* cvd
* cvfwd
* CVODS
* saphostexec
* saposcol
* sapstartsrv
* avagent
* avscc
* DellSystemDetect
* EnterpriseClient
* VeeamNFSSvc
* VeeamTransportSvc
* VeeamDeploymentSvc

The ransomware does not encrypt files in the following directories:

* system volume information
* intel
* $windows.\~ws
* application data
* $recycle.bin
* mozilla
* $windows.\~bt
* public
* msocache
* windows
* default
* all users
* tor browser
* programdata
* boot
* config.msi
* google
* perflogs
* appdata
* windows.old

It has the following file name exclusion list:

* desktop.ini
* autorun.inf
* ntldr
* bootsect.bak
* thumbs.db
* boot.ini
* ntuser.dat
* iconcache.db
* bootfont.bin
* ntuser.ini
* ntuser.dat.log

It also skips the encryption on files with these extensions:

* themepack
* nls
* diagpkg
* msi
* lnk
* exe
* cab
* scr
* bat
* drv
* rtp
* msp
* prf
* msc
* ico
* key
* ocx
* diagcab
* diagcfg
* pdb
* wpx
* hlp
* icns
* rom
* dll
* msstyles
* mod
* ps1
* ics
* hta
* bin
* cmd
* ani
* 386
* lock
* cur
* idx
* sys
* com
* deskthemepack
* shs
* ldf
* theme
* mpa
* nomedia
* spl
* cpl
* adv
* icl
* msu

The following settings are also enabled according to the config file:

* Network Discovery
* Self Propagation
* Set Wallpaper
* ESXi VM Kill
* ESXi VM Snapshot kill

BlackCat also contains a “self propagation” functionality (worm), by using [PsExec](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec) and compromised credentials specified in the configuration. The PsExec binary is encrypted and stored within the ransomware executable.

There’s also an option named “drag-and-drop”, which creates a batch file that can be used to execute the ransomware. The content of this file is decrypted at runtime.

Additional commands ran by BlackCat:

1. **Get device UUID**\
   “C:\Windows\system32\cmd.exe” /c “wmic csproduct get UUID”\

2. **Stop IIS service**\
   “C:\Windows\system32\cmd.exe” /c “iisreset.exe /stop”\

3. **Clean shadow copies**\
   “C:\Windows\system32\cmd.exe” /c “vssadmin.exe Delete Shadows /all /quiet”\
   “C:\Windows\system32\cmd.exe” /c “wmic.exe Shadowcopy Delete”\

4. **List Windows event logs names and try to clear them all.**\
   “C:\Windows\system32\cmd.exe” /c “wevtutil.exe el”\
   “C:\Windows\system32\cmd.exe” /c “wevutil.exe cl \”\<NameHere>\”

In this attack, we noticed that the attacker listed all the logs with the correct binary (wevtutil), but there’s a typo in the commands that actually clear the logs (wevutil). In other words, the attacker failed to clean the Windows event logs.

This ransomware encrypts files using AES or ChaCha20 depending on the configuration, and the key used to encrypt the file is encrypted with a public RSA key contained within its configuration.&#x20;

Once done, the extension defined in the configuration is appended to encrypted files and, like other ransomware, BlackCat created the ransom note with information about the attack and contact instructions.

If enabled in the configuration, the ransomware also changes the user’s wallpaper with the following message.

### BlackCat’s Website

Like other RaaS groups operating in the double-extortion scheme, BlackCat maintains a website hosted on the deep web where they leak stolen data if the ransom isn’t paid by the victims.

They are likely the first ransomware group that allows you to search leaked data through keywords, even supporting wildcards.

### Conclusions

BlackCat and other Ransomware-as-a-Service (RaaS) groups often exploit basic flaws in security policies and network architecture to infect as many devices as possible, stealing and encrypting data to extort organizations and individuals. As demonstrated in this analysis, these groups often use legitimate tools throughout the attack, such as PsExec.

We strongly recommend companies revisit password policies and avoid using default passwords for new accounts. Technologies such as Microsoft LAPS can help to generate unique passwords for local administrator accounts. Implementing a security policy to enforce multi-factor authentication and using strong passwords for domain accounts is also recommended.&#x20;

Implementing strong monitoring and blocking known tools like PsExec can also help the security of your organization. User training is also strongly recommended as social engineering could be exploited by these groups to gain access to networks. Lastly, we also recommend using a secure web gateway to protect your network against malware and data exfiltration.
