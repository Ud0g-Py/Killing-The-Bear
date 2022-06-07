# Yara

## [VK\_Intel (ADVIntel)](https://twitter.com/VK\_Intel)

```
rule crime_win64_blackcat_rust_ransomware
{
   meta:
  	description = "Detects BlackCat/AlphaV Windows x64 RUST Ransomware"
  	author = "@VK_Intel"
  	date = "2022-06-07"
  
   strings:
 
  	// RUST SETUP
  	$r0 = "app.rs" ascii fullword wide
 
  	// RUST RANSOMWARE INJECT
  	$func0 = "explorer.exe" ascii fullword wide
  	$func1 = "ntdll.dll" ascii fullword wide
  	// RUST LOCKER reference lib 
  	$func2 = "locker " ascii fullword wide
 
   condition:
  	( uint16(0) == 0x5a4d and $r0 and
     	( all of ($func*) )
  	)
}
 
rule crime_lin64_blackcat_rust_ransomware
{
   meta:
  	description = "Detects BlackCat/AlphaV RUST Linux/Debian x64 ESXI Ransomware"
  	author = "@VK_Intel"
  	date = "2022-06-07"
  
   strings:
 
  	// RUST SETUP
  	$r0 = "app.rs" ascii fullword wide
 
  	// RUST RANSOMWARE INJECT
  	$func0 = "/vmfs/volumes" ascii fullword wide
  	$func1 = "esxcli" ascii fullword wide
  	// RUST LOCKER reference lib 
  	$func2 = "locker " ascii fullword wide
 
   condition:
  	( uint16(0) == 0x5a4d and $r0 and
     	( all of ($func*) )
  	)
}
```
