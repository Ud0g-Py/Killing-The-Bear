---
description: '2023-04-28'
---

# (yara) apt\_APT28\_Phishing\_webpage\_Ubiquity

```
rule apt_APT28_Phishing_webpage_Ubiquity {
    meta:
        id = "324e9b6f-45eb-4cb4-bca6-9012edc7170c"
        version = "1.0"
        intrusion_set = "APT28"
        description = "Detects APT28 phishing page sending to Ubiquity devices"
        source = "SEKOIA"
        creation_date = "2023-04-28"
        classification = "TLP:WHITE"
    strings:
        $ = "req.open(\"GET\", \"http://"
        $ = "/captcha\", true);"
        $ = "[3].style.backgroundImage="
        $ = ".html?usr="
        $ = "full.forEach(element=>setInp"
    condition:
        filesize < 100KB and 
        3 of them 
}
```
