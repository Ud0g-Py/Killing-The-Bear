# Mitre

* **Tactic: TA0001 - Initial Access**
  * **Technique: T1566 - Phishing**
    * **Reasons:**
      * "In all observed cases, the actor utilized spear phishing techniques. Emails impersonating embassies of European countries were sent to selected personnel at diplomatic posts." (2023-04-13)
      * "The correspondence contained an invitation to a meeting or to work together on documents. In the body of the message or in an attached PDF document, a link was included purportedly directing to the ambassador's calendar, meeting details or a downloadable file." (2023-04-13)
  * **Technique: T1566.001 - Phishing: Spearphishing Attachment**
    * **Reasons:**
      * The main entry vector for APT29 is email. Using this input vector, attackers attach a PDF with a link that will download an ISO. (2023-05-25)
 

* **Tactic: TA0002 - Execution**
  * **Technique: T1218.011 - System Binary Proxy Execution: Rundll32**
    * **Reasons:**
      * "Use of a technique called “DLL Sideloading” that uses a non-malicious, digitally signed executable file to start-up the actor’s tools." (2023-04-13)\

* **Tactic: TA0005 - Defense Evasion**
  * **Technique: T1027.002 - Obfuscated Files or Information: Software Packing**
    * **Reasons:**
      * "Campaigns observed in the past linked to “NOBELIUM” and “APT29” used .ZIP or .ISO files to deliver the malware. During the campaign described above, .IMG files were also used in addition to the aforementioned file formats." (2023-04-13)
  * **Technique: T1070.006 - Indicator Removal on Host: Timestomp**
    * **Reasons:**
      * "In the course of the described campaign, three different versions of the ENVYSCOUT tool were observed, progressively adding new mechanisms to hinder analysis." (2023-04-13)
  * **Technique: T1036 - Masquerading**
    * **Reasons:**
      * "The actor used various techniques to get the user to launch the malware. One of them was a Windows shortcut (LNK) file pretending to be a document but actually running a hidden DLL library with the actor's tools." (2023-04-13)
      * "At a later stage of the campaign, the name of the executable file contained many spaces to make the exe extension difficult to spot." (2023-04-13)\
  * **Technique: T1055 - Process Injection**
    * **Reasons:**
      * The injection process is more sophisticated, the injection will be triggered by modifying the .text section of legitimate libraries. (2023-05-25)
      * The malware selects the DLL used for the injection as follows: First, it uses the system time as a seed to apply a series of arithmetic operations on it. The result will be the seed of the next DLL to be checked. (2023-05-25)
  

* **Tactic: TA0008 - Lateral Movement**
  * **Technique: T1210 - Exploitation of Remote Services**
    * **Reasons:**
      * "The actor behind the espionage campaign prefers to use vulnerable websites belonging to random entities." (2023-04-13)\

* **Tactic: TA0011 - Command and Control**
  * **Technique: T1071.001 - Application Layer Protocol: Web Protocols**
    * **Reasons:**
      * "The SNOWYAMBER and QUARTERRIG tools were used as so-called downloaders. Both tools sent the IP address as well as the computer and user name to the actor." (2023-04-13)
