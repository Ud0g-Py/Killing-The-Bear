# Pseudocode Powershell phishing mocky.io

```
IF
    (PowerShell script execution detected) AND
    (script contains variable `$command` with value `'run'`) AND
    (script contains variable `$version` with value `'v3'`) AND
    (script contains variable `$scheme` with value `'http://'`) AND
    (script contains variable `$url` constructed using values of `$scheme`, `$command`, `$version`, and `$updateid` variables and domain `'.mocky[.]io/'`) AND
    (script uses `DownloadString` method of `System.Net.WebClient` class to download content from constructed `$url`)
THEN
    Alert: "Suspicious PowerShell script execution detected"
```
