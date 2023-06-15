# Microsoft 365 Defender - Scheduled task creation, command execution and C2 communication

```
DeviceProcessEvents 
| where Timestamp  > ago(14d) 
| where FileName =~ "schtasks.exe"  
| where (ProcessCommandLine  contains "splservice" or ProcessCommandLine contains "spl32") and 
(ProcessCommandLine contains "127.0.0.1" or ProcessCommandLine contains "2>&1")
```
