# Microsoft 365 Defender - Check for WMIExec Impacket activity with common Cadet Blizzard commands

```
DeviceProcessEvents
| where InitiatingProcessFileName =~ "WmiPrvSE.exe" and FileName =~ "cmd.exe"
| where ProcessCommandLine matches regex "2>&1"
| where ProcessCommandLine has_any ("get-volume","systeminfo","reg.exe","downloadfile","nslookup","query session","route print")
```
