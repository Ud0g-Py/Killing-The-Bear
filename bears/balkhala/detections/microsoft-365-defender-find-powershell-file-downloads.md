# Microsoft 365 Defender - Find PowerShell file downloads

```
DeviceProcessEvents
| where FileName == "powershell.exe" and ProcessCommandLine has "DownloadFile"
```
