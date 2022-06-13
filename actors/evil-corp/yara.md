---
cover: ../../.gitbook/assets/Killing The Bear - Evil Corp.png
coverY: 344.9027237354086
---

# Yara

## Mandiant

### LOCKBIT YARA rule detects PE files with strings related to LOCKBIT v1 ransom notes

```
rule LOCKBIT_Note_PE_v1
{
    strings:

        $onion = /http://lockbit[a-z0-9]{9,49}.onion/ ascii wide
        $note1 = "restore-my-files.txt" nocase ascii wide			
        $note2 = /lockbit[_-](ransomware|note).hta/ nocase ascii wide
        $v2 = "LockBit_2_0_Ransom" nocase wide

    condition:

        (uint16(0) == 0x5A4D) and (uint32(uint32(0x3C)) == 0x00004550)
        and $onion
        and (all of ($note*)) and not $v2
}
```

### LOCKBIT YARA rule detects PE files with strings related to LOCKBIT 2.0 ransom notes

```
rule LOCKBIT_Note_PE_v2
{
    strings:

        $onion = /http://lockbit[a-z0-9]{9,49}.onion/ ascii wide
        $note1 = "restore-my-files.txt" nocase ascii wide
        $note2 = /lockbit[_-](ransomware|note).hta/ nocase ascii wide
        $v2 = "LockBit_2_0_Ransom" nocase wide

    condition:

        (uint16(0) == 0x5A4D) and (uint32(uint32(0x3C)) == 0x00004550) and all of them
}
```

## SentinelLabs

### CryptOne:generic

```
rule CryptOne
{
    meta:
        Author = "@Tera0017/@SentinelOne"
        Family = "Evil Corp Packer-CryptOne"
    strings:
        $x86_code1 = {68 FC 4A 06 00 68 F4 E0 01 00 E8}
        $x86_code2 = {6A 15 E8 [4] 83 C4 04 A3 [4] 68 45 7E 00 00}
        $x86_code3 = {83 C4 08 8B 55 ?? 8B 45 ?? 8D 8C 10 [4] 89}
        $x64_code1 = {C7 ?? ?? ?? 05 0D 00 00}
        $x64_code2 = {48 03 44 24 48 48 03 44 24 48 48 03 44 24 48 48 03 44 24 48}
        $x64_code3 = {41 8D 84 03 ?? ?? 00 00}
        $str1 = "\\{aa5b6a80-b834-11d0-932f-00a0c90dcaa9}"
        $str2 = "\\{b196b287-bab4-101a-b69c-00aa00341d07}"
    condition:
        (all of ($x64*)) or (all of ($x86*)) or (any of ($str*) and (2 of ($x64*) or 2 of ($x86*)))
}
```

### CryptOne 111111 version (Hades/Phoenix/PayloadBIN)

```
rule CryptONE_1111111Version
{
    meta:
        Author = "SentinelLabs"
        Family = "Evil Corp CryptOne"
    strings:
        $str1 = "111111111\\{aa5b6a80-b834-11d0-932f-00a0c90dcaa9}" wide ascii
        $str2 = "111111111\\{b196b287-bab4-101a-b69c-00aa00341d07}" wide ascii
    condition:
        any of them
}
```

### Hades Ransomware section name .obX0

```
import "pe"
rule hades_section_name
{
    meta:
        Author = "SentinelLabs"
        Family = "Evil Corp Hades"
    condition:
        (int16(0) == 0x5A4D) and (for any i in (0..pe.number_of_sections -1): (pe.sections[i].name == ".obX0"))
}
```

### PayloadBIN digital certs

```
rule PayloadBin_digital_cert
{
    meta:
        Author = "SentinelLabs"
        Family = "Evil Corp PayloadBIN digital cert signature"
    strings:
        $signer1 = "TAKE CARE SP Z O O"
        $serial1 = {00 98 9A 33 B7 2A 2A A2 9E 32 D0 A5 E1 55 C5 39 63}
    condition:
        (int16(0) == 0x5A4D) and ( ($signer1) and ($serial1))
}
```

## Nils Kuhnert

### SocGholish Loaders

```
rule MAL_JS_SocGholish_Mar21_1 : js socgholish
{
    meta:
        description = "Triggers on SocGholish JS files"
        author = "Nils Kuhnert"
        date = "2021-03-29"
        hash = "7ccbdcde5a9b30f8b2b866a5ca173063dec7bc92034e7cf10e3eebff017f3c23"
        hash = "f6d738baea6802cbbb3ae63b39bf65fbd641a1f0d2f0c819a8c56f677b97bed1"
        hash = "c7372ffaf831ad963c0a9348beeaadb5e814ceeb878a0cc7709473343d63a51c"
    strings:
        $try = "try" ascii
        $s1 = "new ActiveXObject('Scripting.FileSystemObject');" ascii
        $s2 = "['DeleteFile']" ascii
        $s3 = "['WScript']['ScriptFullName']" ascii
        $s4 = "['WScript']['Sleep'](1000)" ascii
        $s5 = "new ActiveXObject('MSXML2.XMLHTTP')" ascii
        $s6 = "this['eval']" ascii
        $s7 = "String['fromCharCode']"
        $s8 = "2), 16)," ascii
        $s9 = "= 103," ascii
        $s10 = "'00000000'" ascii
    condition:
        $try in (0 .. 10) and filesize > 3KB and filesize < 5KB and 8 of ($s*)
}
```
