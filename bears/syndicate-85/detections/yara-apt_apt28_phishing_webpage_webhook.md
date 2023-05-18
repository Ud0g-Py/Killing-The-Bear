---
description: '2023-04-28'
---

# (yara) apt\_APT28\_Phishing\_webpage\_webhook

```
rule apt_APT28_Phishing_webpage_webhook {
    meta:
        id = "46adc67b-8bcc-4cba-a480-502c7eb433a3"
        version = "1.0"
        intrusion_set = "APT28"
        description = "Detects webhook APT28 phishing page"
        source = "SEKOIA"
        creation_date = "2023-04-28"
        classification = "TLP:WHITE"
    strings:
        $ = "location.replace(\"http"
        $ = "name=location.search.split('=')[1];"
        $ = "req.send(JSON.stringify({login: name, pass: pass,"
    condition:
        filesize < 20KB and
        all of them 
}
```
