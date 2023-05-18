---
description: '2023-04-28'
---

# (yara) apt\_APT28\_UKRNET\_2FA\_Bypass\_Python\_Script

```
rule apt_APT28_UKRNET_2FA_Bypass_Python_Script {
    meta:
        id = "b8e4418c-1b39-4f78-b2ec-a0b85e4e7ca6"
        version = "1.0"
        intrusion_set = "APT28"
        description = "Detects APT28 Python script used to bypass 2FA on UKR.NET"
        source = "SEKOIA"
        creation_date = "2023-04-28"
        classification = "TLP:WHITE"
    strings:
        $ = "debug('f_res ok')"
        $ = "make_response('BAD')"
        $ = "make_response('NOOP')"
        $ = "debug('task='+str(task))"
        $ = "f.write(str_cookie"
        $ = "if stop==True:"
        $ = "with open(login+"
    condition:
        filesize < 5KB and 
        3 of them 
}
```
