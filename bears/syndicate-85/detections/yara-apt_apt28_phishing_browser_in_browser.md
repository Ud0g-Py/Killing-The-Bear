---
description: '2023-04-28'
---

# (yara) apt\_APT28\_Phishing\_Browser\_in\_Browser

```
rule apt_APT28_Phishing_Browser_in_Browser {
    meta:
        id = "e997b893-3120-4121-a813-281abfaa59c8"
        version = "1.0"
        intrusion_set = "APT28"
        description = "Detects APT28 Browser in Browser phishing page"
        source = "SEKOIA"
        creation_date = "2023-04-28"
        classification = "TLP:WHITE"
    strings:
        $ = "U2FsdGVkX19nY7"
        $ = "').innerHTML=CryptoJS.AES.decrypt(document.getElementById("
        $ = ").toString(CryptoJS.enc.Utf8)"
        $ = "function sh()"
        $ = "$(\"#clickme\")[0].style.display='none'"
        $ = "<span id="domain-name">"
    condition:
        4 of them and 
        filesize < 500KB
}
```
