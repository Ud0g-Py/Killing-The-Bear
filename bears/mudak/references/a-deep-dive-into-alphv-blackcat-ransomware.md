---
description: '2022-07-19'
---

# A Deep Dive Into ALPHV/BlackCat Ransomware

### **A Deep Dive Into ALPHV/BlackCat**

### **Ransomware**

**Prepared by: Vlad Pasca, Senior Malware & Threat Analyst**

## **Executive summary**

ALPHV/BlackCat is the first widely known ransomware written in Rust. The malware must run with an access token consisting of a 32-byte value (--access-token parameter), and other parameters can be specified. The ransomware comes with an encrypted configuration that contains a list of services/processes to be stopped, a list of whitelisted directories/files/file extensions, and a list of stolen credentials from the victim environment. It deletes all Volume Shadow Copies, performs privilege escalation using the CMSTPLUA COM interface, and enables “remote to local” and “remote to remote” symbolic links on the victim’s machine.

The files are encrypted using the AES algorithm, with the AES key being encrypted using the RSA public key contained in the configuration. The extension of the encrypted files is changed to uhwuvzu by the malware.

## **Analysis and findings**

SHA256: 847fb7609f53ed334d5affbb07256c21cb5e6f68b1cc14004f5502d714d2a456

The malware can run with one of the following parameters:

<figure><img src="https://lh3.googleusercontent.com/T5tLnfckSL_GjJhY9ZVYZN8z6iIM6Iw7LlYeyGxKi95wEfowxubPVmbQUWl67CT5XgE1zsUJZ5nfWc55qrf6R6168fahX7l1U7MMaG2HTLWqxGHntZ8YdiyJqUoWnWtxCG_kH9PzvCfte0BlMg" alt=""><figcaption></figcaption></figure>

**Figure 1**

Whether the ransomware is running with no parameters or with an invalid access token, an error message is displayed:

<figure><img src="https://lh4.googleusercontent.com/PZA_QEkSk4NmWtcr6uQtKTKuC3W23t0AxuCKuAk1Q0NGDjt7XAo8nacZ7OCHG-mzRzXVlnXYrG6_3wjL1FyewWyQLx_1XMdJnlMDNkZiKXd1Ab325kQmzQu_T5f93MKoU_JQkOKxlT2pNCZsHA" alt=""><figcaption></figcaption></figure>

**Figure 2**

By performing the dynamic analysis, we’ve found that the access token must be a 32-byte value that is not unique.

The binary registers a new top-level exception handler via a function call to SetUnhandledExceptionFilter:

<figure><img src="https://lh5.googleusercontent.com/hUXNKdIXbRp-w-A2ZLhrpLHauSeCWKoEVbfJHAK8HF_YVPl7wfSZDUDh5kQ_dl5qZmU9KvgUy1-Kr_P1GASWjim29XhjkCt0U_NXJY2KR4q-GX9nl48gyVjdWq7E6ykSAzJGD_nOVY2PaWOdYQ" alt=""><figcaption></figcaption></figure>

**Figure 3**

The AddVectoredExceptionHandler API is utilized to register a vectored exception handler:

<figure><img src="https://lh4.googleusercontent.com/mIvNYtcap3-57yogEpdYvkVQ3cIZ01WoM8-gM6iE1PeMRLJJI0DpIy647QsneHPxsxoUkCKfYVWgrnASR_7YI9RLGKf4EgSQh6R5LQd4kUiBPIOPC1F6Hs9R1T2b6np3Lqr0Xqq0UD1Obd8n7g" alt=""><figcaption></figcaption></figure>

**Figure 4**

The executable retrieves the command-line string for the process using the GetCommandLineW function:

<figure><img src="https://lh5.googleusercontent.com/g95zds0IYOV0GfnxOY7hwJdb57OvD-3sGSF48mwnAv7XQla9wU5mqG2RclmR513Uk6mnaKE5lUoMQWnpDA2RN_-LeDKkSt5oB4m4BHvV1ALjnb8yJpuI0xvC4HdvwbNHQQ398WkDJJUrPAnxgw" alt=""><figcaption></figcaption></figure>

**Figure 5**

BlackCat opens the "SOFTWARE\Microsoft\Cryptography" registry key by calling the RegOpenKeyExW routine (0x80000002 = **HKEY\_LOCAL\_MACHINE**, 0x20019 = **KEY\_READ**):

<figure><img src="https://lh5.googleusercontent.com/XuCXk3V94x0cNenUGDdMYjNVoKGM8vTnh_C7nF4uESksd2c_Er9eL6_dpAeBmBZBmQiAO0TeHUN0p99_iEOMRWhIyNLyX_t702P2A8exDjd06tMGEKuXg4K8LrhI1EZS05OB1MeGn_meh8ivYA" alt=""><figcaption></figcaption></figure>

**Figure 6**

The binary extracts the MachineGUID value from the registry:

<figure><img src="https://lh3.googleusercontent.com/k5XnOWyF2uocs3tdg0-mLnoenKlzpYTYWiy88b9-hNqe4vcsXYU8W7KTvORER10lF7J_Lgvzk5n2I9WtVvOqHHZc-tiWIZoPPTtfSHjV6rKpYRT-iVWFkzoYNemGuyhtEUt04_D8GfXNn79epQ" alt=""><figcaption></figcaption></figure>

**Figure 7**

The malicious process searches for cmd.exe in the current directory and then in the System32 directory via a function call to CreateFileW (0x7 = **FILE\_SHARE\_DELETE** | **FILE\_SHARE\_WRITE** | **FILE\_SHARE\_READ**, 0x3 = **OPEN\_EXISTING**, 0x2000000 = **FILE\_FLAG\_BACKUP\_SEMANTICS**):

<figure><img src="https://lh6.googleusercontent.com/SxKsHpT0VAcGotxwy_uQdFF7HLboRZZHmg8pPBD7ILp7r5YmzjJ98LYBnqyeH0FxXRLnJcXbgH6H6FpB4ArwdLiOL6ZdbP9fAxImqA2Ok_qdqtE57sq2NoeeRe7Y4SkFPAmXhC2FF7jRl-LpCw" alt=""><figcaption></figcaption></figure>

**Figure 8**

The executable generates 16 random bytes by calling the BCryptGenRandom API (0x2 = **BCRYPT\_USE\_SYSTEM\_PREFERRED\_RNG**):

<figure><img src="https://lh4.googleusercontent.com/VRtv8gXfKDLl7vKPH2r46SqwiPbiHladjO8jrUNg35bfiFZsV2WX3RqbIrMO5hGWfdMYUflkKaDai0G1NGFMyMW2dCvOja-QDpkdcJQgY-dphu4wVFG9u7Om-C3FCl_IgikO2rnO8yDTeSNagA" alt=""><figcaption></figcaption></figure>

**Figure 9**

A named pipe whose name contains the current process ID and random bytes generated above is created using CreateNamedPipeW (0x40080001 = **FILE\_FLAG\_OVERLAPPED** | **FILE\_FLAG\_FIRST\_PIPE\_INSTANCE** | **PIPE\_ACCESS\_INBOUND**, 0x8 = **PIPE\_REJECT\_REMOTE\_CLIENTS**):

<figure><img src="https://lh4.googleusercontent.com/Uvd9APu57JZJoo8BTlF5_HwiuFOroJICOTHZZZLJW7SiqBrQFRKF-UJIgdfLdH0vu8onMjN4kVFl2Kt-Gs7-FsEVJZa8U0kE7lT0VzYGzmtyU0GwON5wOq-FMxlqkIXPbgFQ-jBXJJEtd0Nf2w" alt=""><figcaption></figcaption></figure>

**Figure 10**

The process opens the named pipe for writing using the CreateFileW routine (0x40000000 = **GENERIC\_WRITE**, 0x3 = **OPEN\_EXISTING**):

<figure><img src="https://lh6.googleusercontent.com/ULJ-vWfkDYmgMIq73nL3XKXZq1aSnxAfZ3HIsg-ogp8a5F1YO43UHE-1V2CLOraglwXVKc2hWWDt-4Wo9Ic-tbOfwCkdwuqGe4_qpebHv0zR6-0e44iv_RD17IXto_CvhDHpEwst-VbCYkLizg" alt=""><figcaption></figcaption></figure>

**Figure 11**

The ransomware creates a read and a write named pipe, respectively.

The wmic process is used to extract the UUID (0x08000400 = **CREATE\_NO\_WINDOW** | **CREATE\_UNICODE\_ENVIRONMENT**):

<figure><img src="https://lh6.googleusercontent.com/2KVq4oXVyVVIZODSvjABcc_8UkPQPgq5fAew_e3e_ro3n2OBQbWPi-SvB2KSK-Hzb712zIBLG9Rg7SXyURTl-eMIFXGPDxxanCe8f7HQXCf4RZkUL-le-RCON8PiM8AGlzYUQYFvxwVcFHj3-Q" alt=""><figcaption></figcaption></figure>

**Figure 12**

The CreateEventW API is utilized to create two unnamed event objects:

<figure><img src="https://lh6.googleusercontent.com/_wKiYFJxBfwPzMa_lFYtoTK_IJE5Jt5wYdErMtGcChDNnIlDmuQPA9DOr04JRpGm2weaZrIBE8R1HQXtsuwww_XZdOoVJPsYZ42vNxon2yEvnDkpFzrHRQk5q1EuxcVLM_hpKQ71ZsCBKRUb1A" alt=""><figcaption></figcaption></figure>

**Figure 13**

The binary waits until the event objects are in the signaled state by calling WaitForMultipleObjects:

<figure><img src="https://lh5.googleusercontent.com/DTeKMxBGlh9n2lLEwOkx4AGpHcB7efX2RAXuasSuJc4CZZnZI0LFjjeVqJjl1RkvF1tX9fszzZPMgrjPBUxQUZTNZSi_oi9G1aVut4fJkoO7u0yJyeSLKh1ZWzuaFYEFbPrUJrvG4sbK_OiL1w" alt=""><figcaption></figcaption></figure>

**Figure 14**

The output of the above process is read from the named pipe using the ReadFile routine:

<figure><img src="https://lh6.googleusercontent.com/AGQ4pHpBpBVD3VeICiKu1iWyU37gpOkmnaZ8i2rXclMYaTpz-6OKfm70joSfVOLoeQYKBlOgTkanEPpJBpdQVw6-uY9HFHfGf3ASG3zl9xInWaFnr-Y6dM5SIjn063hzVVq-EehVap43zEB2fA" alt=""><figcaption></figcaption></figure>

**Figure 15**

The malware creates multiple threads by calling the CreateThread function (0x00010000 = **STACK\_SIZE\_PARAM\_IS\_A\_RESERVATION**):

<figure><img src="https://lh4.googleusercontent.com/KctKkITH2habG7Pmy-B_AihXD03KUkLDl68CxuMtXj4EBz7Fm1Q-ajtnd9ckRDJyxodZ18uPwYp6UlLhw6NJcrqkAmfiKzOHoh8RvsZ8ZQl5d4cYz3Fs9tTLe4003Ufkfe93mWWc4fFgfOsrKQ" alt=""><figcaption></figcaption></figure>

**Figure 16**

The content of the ransom note and the text that will appear on the Desktop Wallpaper are decrypted by the ransomware:

<figure><img src="https://lh4.googleusercontent.com/9wbG8UId_1l9BP5HCuoP-6jp0a8rzgQEguQDNtQIlaEAF6OA2uVRCbchMvierAh_wYZNtrzvz6Re67CSJ6nceGBe9J8yg_q7kw81HgAYR4IPNdRktOgdrmfu6Vz3JV9U4YQ6D4gqltIS6C1bkA" alt=""><figcaption></figcaption></figure>

**Figure 17**

<figure><img src="https://lh4.googleusercontent.com/4Ajp0y4NpABPnefHWGoNvKzsXSFxYmYQgFT4Ft7s3LvOJ2HsvrI69f3E5clI38577s0-t_u_XQlvjB5nwm6zjCU8KinsVgEZBw7p8voNoLIQmdFGrCPnp0ScLeNbdvBktOk2qqnGMfYvLx863w" alt=""><figcaption></figcaption></figure>

**Figure 18**

The malicious binary obtains information about the current system via a function call to GetSystemInfo:

<figure><img src="https://lh5.googleusercontent.com/8Kzculj-zGFEGzVlzD0UlaUkys_jBnyOAK53wgdA8dZH3__Shls1NOihYYKMbpPuSjS4zLYAli7NRAp9DXq7KdWOzs4h85DihMXr2LgOkLqpjnQ_L64CwFnoCLxepy-aU9xEGhTz_VyGrzieEQ" alt=""><figcaption></figcaption></figure>

**Figure 19**

There is a call to SHTestTokenMembership that verifies whether the user token is a member of the Administrators group in the built-in domain (0x220 = **DOMAIN\_ALIAS\_RID\_ADMINS**):

<figure><img src="https://lh4.googleusercontent.com/VbxZ3BVfqeD1FuR7PrNFiiFpSl9kh9vc4MwnWB_voT21uGmv9ALDGQIAqqNVjZ5KS_d5ny6Y5x6VyTNHuuxwzeaIU4Q4NXuGE00fsmTjM2qUc-vuMOE8hVWOMovizlYbyjdR73QTkIfWN_Ln7Q" alt=""><figcaption></figcaption></figure>

**Figure 20**

The process opens the access token associated with the current process (0x80000000 = **GENERIC\_READ**):

<figure><img src="https://lh6.googleusercontent.com/AWi2QbcBvNiJH8ohTHAzISvsW9kguetGfKySszkLCJRPEcdu8RAlIB3SWlk1fh41mRJZi6rZEtHpoKNe1-nOyfAENs67_w5jGmxSm5TEprIRKDfSkDVvSDnEKIJFe8_hQpna0Ig5_0wKH9wxEw" alt=""><figcaption></figcaption></figure>

**Figure 21**

BlackCat extracts a TOKEN\_GROUPS structure containing the group accounts associated with the above token using the NtQueryInformationToken function (0x2 = **TokenGroups**):

<figure><img src="https://lh4.googleusercontent.com/KbGaznq88FmWpzQ4d9W6-ms6zuUiTxG1K1-gPA6HsT8X9UG6QOCgxyFiyNVEZUGXH8wS2_6S1HrcB3h6hpv4895gBucd9wP9DvyiPeK6dZUKyFTjVZ0GG-b5RvpQIgF_RQoGpd7gHBgwHxbftQ" alt=""><figcaption></figcaption></figure>

**Figure 22**

The OpenProcess API is utilized to open a local process object (0x438 = **PROCESS\_QUERY\_INFORMATION** | **PROCESS\_VM\_WRITE** | **PROCESS\_VM\_READ** | **PROCESS\_VM\_OPERATION**):

<figure><img src="https://lh5.googleusercontent.com/d_llr78YPovMUmwlKUoepev_7984BdjMF57PzBICm5eTPQx5fTLy7NB-hs4cr3HTKkXcHvmoUQbAjHbAMCbZb8hTYywIeGEqshoDH9awuVeAoTK99FEev1yFvP5gjikaRiAqfAITWgXV8m7WUg" alt=""><figcaption></figcaption></figure>

**Figure 23**

The malicious binary retrieves a pointer to a PEB structure using the ZwQueryInformationProcess routine (0x0 = **ProcessBasicInformation**):

<figure><img src="https://lh5.googleusercontent.com/Iw7vJkD7Qy7Y1K1vWnjI4HIN3NaCozJFkIUI9QMsgdDr8mBNXZ1qDVxlwGCV_VvO_uwkBph1pWpAUZLqGpzm1Ih96nzbOUOMELXT_nSSsfGrr73lyJ0-i9hAK2CGsmo5H9sIVBkocuOksDb5Nw" alt=""><figcaption></figcaption></figure>

**Figure 24**

The executable retrieves a pointer to a PEB\_LDR\_DATA structure containing information about the loaded modules in the process and then to the head of a doubly linked list that contains the loaded modules:

<figure><img src="https://lh5.googleusercontent.com/E-crDnHTZLYb42U0lOkRFqTWelixQ-KN5iDX0YmsDAemALLsVQskOZHTZ8pjKkV8BkgKGFmAd-i7QdTftvL2Dqc2cwfxPa6025FeJGAZ0puDremvv6zTKoM08mVQYKo9I_ZsmcXfgOILmTVhjw" alt=""><figcaption></figcaption></figure>

**Figure 25**

<figure><img src="https://lh6.googleusercontent.com/3qGvqkuH9C8tnGKjidYfsU5q7A19PBsUPjECjJp88jU44TwtDMVAuXvs9n7I3jmo0rrrT9q55R57PO1Ytp3vX1u8JSFoXkyyX-y4IW8IIGm6ROM0zIKqPXWMWBfAOO3ulfqyRhHQ9j_6fNfm1Q" alt=""><figcaption></figcaption></figure>

**Figure 26**

The path of the image file for the current process is retrieved using ReadProcessMemory:

<figure><img src="https://lh6.googleusercontent.com/aB9WsXvmIjnWjCfg47ksOvLVjcJK5lfQZWVqqNogY0v76cCUjOZ7kjnFqSzJ7C6gxlemq7gzPCO0Lv6QoA1AcaeDqynDlRshOsPGQtQv7pOb46Aib83ofwgIr6k-ppJvctOFmxAVCteJHT90Ng" alt=""><figcaption></figcaption></figure>

**Figure 27**

## **Privilege escalation via UAC bypass using CMSTPLUA COM interface**

The ransomware initializes the COM library for use by the current thread via a call to CoInitializeEx (0x2 = **COINIT\_APARTMENTTHREADED**):

<figure><img src="https://lh6.googleusercontent.com/w5rYgisjfL7AnAkg0D3cU_RHZL-eSAMeZkV0xVrS8Wau_YRCuvxeVC4wFwS8tFTYAwt4NTZW2W9tjbkknl9dauvAGBJmlcbCfsZhikY3G2ZkXR3bWB9KetbQJ4j9CuTxc6bxmBBjzvNM_9fBLQ" alt=""><figcaption></figcaption></figure>

**Figure 28**

BlackCat ransomware uses the auto-elevated CMSTPLUA interface **{3E5FC7F9-9A51-4367-9063-A120244FBEC7}** in order to escalate privileges:

<figure><img src="https://lh4.googleusercontent.com/EhYrPKx20hiwIrE0UscLGxVMdu6lDCR5H7EfXWEPpN6weN2r5IT4HgPoZnwGEm7QU48FDHwzTcUeGZE1jgY81rt1mEvJFkHragfsHLOkQMvZFsQbxYWntkaoQfBGz3GPIDgfonX1oaeTxwatgQ" alt=""><figcaption></figcaption></figure>

**Figure 29**

The initial executable is spawned with administrative privileges:

<figure><img src="https://lh3.googleusercontent.com/hLfgaD9FePC-dnmEl77SUkWfrIZB-j-VSZr3U7kqqTTanCvYHSNm204acLM4Qww9PB08jjcRpc6gvlH9AkdsTMcxaJlONhBAuxF-6zO4p8HjDie6qnnsDL1Jdsug2C3UuCT6ecHjTrFttGXdMg" alt=""><figcaption></figcaption></figure>

**Figure 30**

The LookupPrivilegeValueW routine is utilized to retrieve the locally unique identifier that represents the following privileges:

* SeIncreaseQuotaPrivilege SeSecurityPrivilege SeTakeOwnershipPrivilege
* SeLoadDriverPrivilege SeSystemProfilePrivilege SeSystemtimePrivilege
* SeProfileSingleProcessPrivilege SeIncreaseBasePriorityPrivilege
* SeCreatePagefilePrivilege SeBackupPrivilege SeRestorePrivilege
* SeShutdownPrivilege SeDebugPrivilege SeSystemEnvironmentPrivilege
* SeChangeNotifyPrivilege SeRemoteShutdownPrivilege SeUndockPrivilege
* SeManageVolumePrivilege SeImpersonatePrivilege SeCreateGlobalPrivilege
* SeIncreaseWorkingSetPrivilege SeTimeZonePrivilege
* SeCreateSymbolicLinkPrivilege SeDelegateSessionUserImpersonatePrivilege

<figure><img src="https://lh5.googleusercontent.com/ML9sz8_uNWqtTfsxrYycGKMN8AbeBGtTuRK9OAo_1xMT8bzjMkgtlEd5y2EDpEFm4y8hoqYC8ikcnNk1mGLGITNR99GkPCK2sNXFLBJpaobBJoo3hJMRtOqy8hpB5WvAEKduEziVEVnZML8bWg" alt=""><figcaption></figcaption></figure>

**Figure 31**

All the above privileges are enabled in the access token using AdjustTokenPrivileges:

<figure><img src="https://lh4.googleusercontent.com/R9LaAGvghNr3kMGPxx8qF88ooqjNmS3tQKd2HXHpv1JlXCU5iRCfypKChTM7M6b-5Jfq9ZHlDiu19C1tC75OflRQ_Zl6qxJAv6EH6Z4U5yxl5cWbHFS5gAVCFPIpMEdIzZJY8ZDh39bG71EhlQ" alt=""><figcaption></figcaption></figure>

**Figure 32**

The binary creates the following processes that enable “remote to local” and “remote to remote” symbolic links on the local machine:

<figure><img src="https://lh6.googleusercontent.com/Ac7gV5uLv4jR-0s99xcrTseF_-dqVc803IjGUHrJTS5glhHM2skSIFWzmFRLOBBxHPpbKXpjGfhpqIx-FYHZl7Hq-1Ib9czHGaDc5geL-9eXUxPD8JZPBXMqnw37jFx19g-gWOt9AOYlaPU3IA" alt=""><figcaption></figcaption></figure>

**Figure 33**

<figure><img src="https://lh4.googleusercontent.com/E_y4xqbhMAbz6C_YPrlzo2dTZG5ZC8NBjDVwS0PqylUb6WH33NeK_MfTtajqavZv1eV-7K_pe48ezFY0NOL16hAbi24S3DcKJ1cq1YVY67TurchRdhqg2JnjwjmNvHxlyRQtw3NUDzYFBPeVNQ" alt=""><figcaption></figcaption></figure>

**Figure 34**

The malware tries to stop the Internet Information service (IIS) using IISReset.exe:

<figure><img src="https://lh6.googleusercontent.com/35wr8ByklOqy0_MN2rdcmxTNaqKp4bbQ5ZqOCvDt6Nsbw1xRSonL8J26F8Tc-UeFzGRw8Cs3HpKZp_Qsc5bwNYxDrriOgWtXaeT8tKnKgvddfGK5-hd1vpd2WlhKjTdb28S6f8jjFIICYrsaAQ" alt=""><figcaption></figcaption></figure>

**Figure 35**

The ransomware deletes all volume shadow copies using the vssadmin.exe utility:

<figure><img src="https://lh4.googleusercontent.com/-x92ifTP2i0KUv3NAiuzfV6zVIrQTtSAiP6evToTMe0im6367MxJpHnHe2XnK6CQ3eXgmXGIv5ReZNcPqK3KPeLI-oq58RSsWT6QDROku3jSAHbffYoxB4KD8-t-7fuCaaLgQ8bmVz0cDnSl0A" alt=""><figcaption></figcaption></figure>

**Figure 36**

There is also a second process that is responsible for deleting all volume shadow copies with wmic:

<figure><img src="https://lh4.googleusercontent.com/NY4tGrQ0RwpI5APqG3I_rtPGxKv6rCS_jsb77JdX6t5P3UJM7NzmaLbFP9Qrbj8phaZ6l22EoKcBXuMjfn2q771266wRffxUGjkHBeNcQBY1Xa7cQpbhbD7KqlsCoW4hwdkEZFATu6-I-8PK2w" alt=""><figcaption></figcaption></figure>

**Figure 37**

Interestingly, the malware runs the following command that is incomplete and returns an error:

<figure><img src="https://lh4.googleusercontent.com/yDod9wO5ECnrhlrnvEfKSqgbHtL-WOF1WGhFKyeoNTn-a3O64gh-LFdIM1RbH4HVxv7LlUwCpCay5Dl-3wz_Kr-MVtmKMEYcM76U-UzTQB3LEePTxj-wywqdlJ--F_o47ae0577UQg_LpQUCqQ" alt=""><figcaption></figcaption></figure>

**Figure 38**

<figure><img src="https://lh3.googleusercontent.com/MUh2OSoZP2jJWDX7PqbWkoVm5vFlD1QxhVA36U0pafqqHLyZ77uImpMIUVFzj86D66Smr2ryP1-DHrm4FtA7nfj9MPc5YTQqfmyW8NvBqabvLWKffE86Ku8gdfxv7c-qfT1GgRDaK_mEIivn6g" alt=""><figcaption></figcaption></figure>

**Figure 39**

The binary disables Automatic Repair using the bcdedit tool:

<figure><img src="https://lh4.googleusercontent.com/YswGp6B0S7Pw2GkIPA6t4aV3OMPxlX2iZeBH2v5YU6XXbbBfsZTg4gjmPovPvhhTVsEzEUTV4B-xg3DumzdZjRdFpi5wewvjx8ooCW6XhEU5Nylemfp40ecS0L16P9NLkUENuy8jlXJkIBRzQw" alt=""><figcaption></figcaption></figure>

**Figure 40**

The ransomware tries to clear all event logs, however, the command is incorrect and returns an error, as highlighted below:

<figure><img src="https://lh3.googleusercontent.com/idrW1B2GTkDc6yCaVOOd8OmUl07OweSyJ1VFJVRcfM4TtH7oJdgHlN-TB-hf0JWasU0fve644B_4Y0jmzf2dZ6tALzcuSy1FNoZVc7b725eW1oB0QitJnRoqUESMmkpeo6wPEmPSHTXpt23nTA" alt=""><figcaption></figcaption></figure>

**Figure 41**

<figure><img src="https://lh3.googleusercontent.com/DJlxoqJ0zaYXrT_UclwMqPOi75G15mzJuWmuj5iVoaVRrQ-ElFUSlT5Enf1L5TraCBjsvHiyZgIAKsAaWJgrEqg1GbzNrZQGTLrsz5T72ITUDFxmne3-Ge85O6aBCGEpklb42RySRAohViqsag" alt=""><figcaption></figcaption></figure>

**Figure 42**

\
\


## **Killing targeted services**

The binary opens the service control manager database via a function call to OpenSCManagerW (0xF003F = **SC\_MANAGER\_ALL\_ACCESS**):

<figure><img src="https://lh3.googleusercontent.com/rCS_3kVhVMQ8KGCBmIxJQosEZYAvJQsuKYrddUy4sprzgOnWAOF0sRzSBbuVod1NVQy4ik1akD2NInrPRZfgVb4c-OX_FRpN94jUstXw2QM-vErBCkaZ5YWB-7xaKSoYXPJX2k9gvd9EgVCWlw" alt=""><figcaption></figcaption></figure>

**Figure 43**

The process obtains a list of active services using EnumServicesStatusExW (0x30 = **SERVICE\_WIN32**, 0x1 = **SERVICE\_ACTIVE**):

<figure><img src="https://lh5.googleusercontent.com/i9JkwtfWFrOUngyQxgLrxcrk5nBgKbSA30Gat86yDaNEbYMGUpGCNdU4eyprdLPER34JajS5qBDkfTflmjnZFd28mfJBNSUMqO8LloaRbVUkt3CQSgv84omz8mmxa-wPC7pl_FjZ_PQHYFjUkQ" alt=""><figcaption></figcaption></figure>

**Figure 44**

The malware targets the list of services from the kill\_services element in the BlackCat configuration.

A targeted service is opened by calling the OpenServiceW routine (0x2c = **SERVICE\_STOP** | **SERVICE\_ENUMERATE\_DEPENDENTS** | **SERVICE\_QUERY\_STATUS**):

<figure><img src="https://lh6.googleusercontent.com/eOjCAbTHBykS3IWa-cm8ZAOXCNsMJUZbV83HNrARaB3B1C3zjk-9dFSnMoErJphFT-W-n_VwxP5V41h-LG4vjJJL-60hDAo6uGQ0X2fxuRvvGk0YpvX432bS_xdu3zVscEo1HTriCJsWOhN7OA" alt=""><figcaption></figcaption></figure>

**Figure 45**

EnumDependentServicesW is utilized to retrieve the active services that depend on the targeted service (0x1 = **SERVICE\_ACTIVE**):

<figure><img src="https://lh5.googleusercontent.com/N9a_dlhMVZh_PxmMaH1eW0DFm-F_HbilbZmeLgtaDPnydj8JZsnPzpbWz-kYnXDyOL_cEaqe2_-YAzk4Q9AHJ3T72Nz3m_oZXsvPppW44Z_YjzyeJ02SHJM37MYQaSjBRtbNXatQX23Y8wyLvw" alt=""><figcaption></figcaption></figure>

**Figure 46**

BlackCat stops the targeted service using the ControlService function (0x1 = **SERVICE\_CONTROL\_STOP**):

<figure><img src="https://lh5.googleusercontent.com/CUXZ7-7WIXLLQO_leW0Or8J5xwCCLWvX80iWvpmDVcwXR8lB42lBQFwveXMjYGYmIHbjb_0V1e0qqoejVxgRYH7gk0CoWLrlcubHuWjyLtG3D_RJSBG9sytYSc7TjJchG0DFyh72VzTxPc30Bg" alt=""><figcaption></figcaption></figure>

**Figure 47**

## **Killing targeted processes**

The executable takes a snapshot of all processes and threads in the system (0xF = **TH32CS\_SNAPALL**):

<figure><img src="https://lh5.googleusercontent.com/GreCDRRhzy3kOM41DOfQfJURCcWGcWVYbx_iMAX8a5rx1QLOqnPKQqr6sdrhHnOw9NRtSuWDfjvw9ZauDyaMJByPUiOPOUcTWBPexNECp7q2Iz51qxmoh_otPnmMZsdA-ivx3bn6AaTqKD0Wpw" alt=""><figcaption></figcaption></figure>

**Figure 48**

The processes are enumerated using the Process32FirstW and Process32NextW APIs:

<figure><img src="https://lh6.googleusercontent.com/DZ_UBLsL0P26T88T5bINhMWLHzOVzBFc2PQUkvHzVKhrrwYBWCFYZURCTcPfdj0ySml4Il_UAGiQPiISrKlAcEGtPgc3KVX7g13BEMuJAtYXvQEvz2t_IvyLfq2-cXjN3sJA29o3vdcxLq5AuQ" alt=""><figcaption></figcaption></figure>

**Figure 49**

<figure><img src="https://lh4.googleusercontent.com/LHeijF6N7UF74fhhAKFYZbQPvVBdVsahFVF4rAiprlPMM1y_t1SHX7omXmF6WqZ32l7yaYPczE1qiQUQx45qBnijNhu0iH-ra1YbVgOT-h7CZQdi31iDGSYYUukCrcIKfZ_0TIo4w4dVO30mnA" alt=""><figcaption></figcaption></figure>

**Figure 50**

The malware targets the list of processes from the kill\_processes element in the BlackCat configuration.

It opens a targeted process using OpenProcess (0x1 = **PROCESS\_TERMINATE**):

<figure><img src="https://lh6.googleusercontent.com/A1lvEDimkPwUgT9uC6PGIjvRfFEjCarpT865rqMyolufpotgXvT-5KCxPT3jehIMGMPOnuUnSnvejbobJLotq2KsHWaTINLx8HI9uasJTmCrxRaPkRiSNoNmabfviEXvqg3XiWlS8L9P9xjWyA" alt=""><figcaption></figcaption></figure>

**Figure 51**

The ransomware terminates the targeted process by calling the TerminateProcess API:

<figure><img src="https://lh3.googleusercontent.com/wN38AjoShq79Ica6aYbAWEbr-PMpFHOumw7uDbjurPvAdgH_0k2DFPz6wdBSO51TznvcT6djP1v6oxYs5ELk8tHFlLH-468vsF8STu5ObWXrnN95o4D0y01TJRYlAXZOtMHKaJCS9-vXQ4fVWw" alt=""><figcaption></figcaption></figure>

**Figure 52**

The binary spawns multiple child processes by adding the “--child” parameter to the command line (see figure 53). The new processes run in the security context of credentials that were specified in the credentials entry from the BlackCat configuration.

<figure><img src="https://lh6.googleusercontent.com/7HrHdMk4RjDEPEGJvuDFUt2YHHVD9PwJYNnxV3ThRki6noJuSpjbUrBqfLJyfXWlEz2s1FHRNG0gKRh7Gg3HLYo2cgxBnAwfH3J9mV-9Uuof0jT-KEjD5Qm1Z5a-PaydBxc2mi-Vo-EXM6C9OQ" alt=""><figcaption></figcaption></figure>

**Figure 53**

The number of network requests the Server Service can make is set to the maximum by modifying “HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters\MaxMpxCt” Registry value:

<figure><img src="https://lh3.googleusercontent.com/1eUXuiApDbxJh93PuvdphuJ_yDHdtGPQ9s-3u7dOc_BVruMOJNJiCvAtovmHHlr7hPeqcDLiTDv3bWy3jcaKoYsFAxnh40wCgp9PvxGOi9xgd1fbvLkfxZe7-ZASXxp976o98f5CnhX4pkmiyg" alt=""><figcaption></figcaption></figure>

**Figure 54**

The malicious process obtains the ARP table using the arp command, as shown below:

<figure><img src="https://lh4.googleusercontent.com/yXFsyIasdn_YvqRJUHGiL_S8iNojP888hICjHpcIwI0svhiCn5Z-qe5FlmH_iW1wHcDUIrP2wmFvNegAGC88iR7RrHxYR1RzLufsFdbJWvkkYNXvevQAd9CL9AH5bTxU95Q2dhuVTNE4UPjOTw" alt=""><figcaption></figcaption></figure>

**Figure 55**

The net use command is utilized to connect to the local computer using different credentials stored in the BlackCat configuration:

<figure><img src="https://lh4.googleusercontent.com/cBHEvG3QlsrJq3AhgJdlVoUQExCw1Ik1uZTDPBOeXX_Sg_b8PnrCjZ4QjLhKU15cfLKpV1EJjZJyVKIy8C1ypmVulha5rVuRIMHq82gljJVbk_ljzgC7LpwQ1Wx7VX01eKNyM48VfsGnrx5qEw" alt=""><figcaption></figcaption></figure>

**Figure 56**

The malware retrieves the currently available disk drives by calling the GetLogicalDrives routine:

<figure><img src="https://lh6.googleusercontent.com/DIULsvKjYfK0f3uY8UTI_95njFMXCbCG9vRZEm7EuFwl5-Pil6CrxPyWte7kDbjaFhSXJS8bunV1jRpIGZTisLioCBcseEHfYfb6nC3ZN7rqLMxHnXzgNaHtNYGUnTJehhDY7L2VqabpT29eUg" alt=""><figcaption></figcaption></figure>

**Figure 57**

The GetDriveTypeW API is utilized to obtain the drive type:

<figure><img src="https://lh4.googleusercontent.com/Dp_iPwbqAKseDv2Cd2bg0WARHlZj1Kf1wmjUwXkmDzx7W1r2qwJ6eQsmOEgJscMpxl6v4faFDZGDxVU8xmsU60Fyd5ZN-eXJgtwB0Xf_TsyxpOFz69rvozNXFUe4dfq2uYKQI5oGZ0CkRxqFTQ" alt=""><figcaption></figcaption></figure>

**Figure 58**

The ransomware starts scanning the volumes on the local machine using FindFirstVolumeW:

<figure><img src="https://lh4.googleusercontent.com/XYk3ZZOcAKSPMAQpvbyQtZiWZSrQhS7nPWQVKa6UQ1_LhN7qFNFnagwFCnimMJcZnPE7RB6ZYziHczW5JDUqPQsq1ohopKu3QsIec86zEkQ8so1lsKPnJR8QBlXrRne85fvtu6_Cqzi_77dgYg" alt=""><figcaption></figcaption></figure>

**Figure 59**

The list of drive letters and mounted folder paths for the above volume is extracted by the malware:

<figure><img src="https://lh4.googleusercontent.com/49gG0tz3r6UFsvxWhM0EfLv_RYBN__mtnVQZoizpvyBl1gIzdiHUZDwL9MoiK52a0vrxmTbgXFsALiNvpDXoz2ObBzWgNvRKs1MO-goCMVwIfgoWypmHnrD8fk_UkA6Go6G9_uCsIEUQoAGiJw" alt=""><figcaption></figcaption></figure>

**Figure 60**

The volume’s enumeration continues by calling the FindNextVolumeW function:

<figure><img src="https://lh6.googleusercontent.com/F1Yk2EnlkKUyXpnM_7m34Hp06LISu1SGRxFoBJQ8i4w75V3GKKrKM_8_2sbeNMKWaWwALB2dsFuJGi1UE2p7utasv4ddKUf-TQgHVohjJtW6-SSrF5vaSnHheijM-jDxiXumHx1zsKMiFDD2Kg" alt=""><figcaption></figcaption></figure>

**Figure 61**

All unmounted volumes are mounted via a function call to SetVolumeMountPointW:

<figure><img src="https://lh6.googleusercontent.com/nigaq5BWpDsNgR0M9aFRhuUwpsKflKhLkzY8hRVT9QIoSwbbfCKhTR7aRtkSkiWsVmcvDhQjCHxqsqp_QPnNu4XxnWvQ6HlybNS4iJTzwp3MvPrLSWRzdrJ48LOyCQ_DKe8FTtJ5DwR55OABUw" alt=""><figcaption></figcaption></figure>

**Figure 62**

BlackCat traverses the file system using the FindFirstFileW and FindNextFileW APIs:

<figure><img src="https://lh4.googleusercontent.com/LzMebt7YYH8kc6zOwbYiEeZ41A5fcHxMAm4cGQPZCE5gmYXKE3MMH6Q-YQ5ACCj29Ps_ebi5CyLJmk0dFp96Lp_ecVKolp5fhhMytcsttNpXqSa0KqpJUMCvIJFq-DOsvOU66f7QjfAJDZiOgA" alt=""><figcaption></figcaption></figure>

**Figure 63**

<figure><img src="https://lh3.googleusercontent.com/nXeOH5YE06-5CjhrO8PTiP6p7L3Jz_RSY3wmPlqZyfYrh2ET85jV65kG9wPXfrc2SHNN8M_gnl7BKZk4S47i_B4INZFJVHHpYLzrmlwI5JK9He9O4ibvGGlmik2m-nIG72O9YB9R9HyR7P-WLg" alt=""><figcaption></figcaption></figure>

**Figure 64**

The BlackCat configuration is stored in JSON form and is decrypted at runtime. It contains:

* the extension appended to the encrypted files
* RSA public key that is used to encrypt the AES encryption key
* ransom note name and content
* stolen credentials specific to the victim’s environment
* encryption cipher: AES
* list of services and processes to be killed
* list of folders, files, and extensions to be skipped
* boolean values that indicate network discovery, lateral movement, setting the Desktop Wallpaper, killing VMware ESXi virtual machines, removing VMware ESXi virtual machine snapshots, excluding VMware ESXi virtual machines from termination

<figure><img src="https://lh6.googleusercontent.com/5o4-fwI2M6FKEvwIIjfoRPL7obVYuXKGlgKhPMB9JjdXr_snYU-Z2V3b3xFeCub50nSRt1agr3RD_avp0Kzu81dWvKiOTEXVtHK3g3nAk5-XXRj5_-B30iVSY87xfyVPUSTgkijne4u2iztDMA" alt=""><figcaption></figcaption></figure>

**Figure 65**

## **Files encryption**

The CreateFileW API is used to open a targeted file (0xC0000000 = **GENERIC\_READ** | **GENERIC\_WRITE**, 0x7 = **FILE\_SHARE\_DELETE** | **FILE\_SHARE\_WRITE** | **FILE\_SHARE\_READ**, 0x3 = **OPEN\_EXISTING**):

<figure><img src="https://lh4.googleusercontent.com/Cz4VnVG-RCUt3k6R5g9E2seGxRSqQIxVpgQ9vuXzdFHJEBoXpwHOJ6tMJHIps-Y7mRoT18p77w0IvDdqnTK_jA7h6iFgM7_um_zqQRzujBtL00Lg5hLvOlujYWdILQOJByr-z7cWOpzAYMNGJQ" alt=""><figcaption></figcaption></figure>

**Figure 66**

The ransom note is created in every traversed directory (0x40000000 = **GENERIC\_WRITE**, 0x7 = **FILE\_SHARE\_DELETE** | **FILE\_SHARE\_WRITE** | **FILE\_SHARE\_READ**, 0x2 = **CREATE\_ALWAYS**):

<figure><img src="https://lh5.googleusercontent.com/GpsqUXutZVt2sBBsM_0gtjBDByD_cozP65Todu1vCfnxaZWTzzIivqhVzHaF-Cx_-M5PboA80WrtU8FGj56asKFcMEjSl8mCwTy5GxouO-qLfTrS6m05lwyifPnVuuxDMF0V5IybLIRnTRqn8w" alt=""><figcaption></figcaption></figure>

**Figure 67**

The ransom note is populated using the WriteFile routine:

<figure><img src="https://lh3.googleusercontent.com/Li52PeKchvTc3qQEUBhxpkltRxwU1PFwfzk6zN8VVKdfEWapG7vGKFWl_hgVGbgeR6eH7AS77hHUfbNn5RGZaDEwd_zJsXNq1Y-sfBiQLSRIuySQlqf-LLItP_4Xofd8MKUODyqGkNQK8cGvmg" alt=""><figcaption></figcaption></figure>

**Figure 68**

<figure><img src="https://lh3.googleusercontent.com/-x1cUhrWE_0Rz07qF2IveDKUgE_wMfiH4G3FXoKdWjG3pOPEPoHaXkJCc8ss3lG4mmlYk6dOpcHJlThPsSe42CxL_HDU_XJ3K_LrhDzuhOg7ErvFlk54oQWc4t-OdY4vv8EczpPRLG21d-gwqQ" alt=""><figcaption></figcaption></figure>

**Figure 69**

The file’s extension is changed using the MoveFileExW function. The renamed file is opened using CreateFileW (0x7 = **FILE\_SHARE\_DELETE** | **FILE\_SHARE\_WRITE** | **FILE\_SHARE\_READ**, 0x3 = **OPEN\_EXISTING**, 0x02000000 = **FILE\_FLAG\_BACKUP\_SEMANTICS**):

<figure><img src="https://lh4.googleusercontent.com/fNdFBi7QTUl5V3aj2LDzVyK3ju8M5wkwJ29Y4FF_oFmsk9YKoaN2gGZdVqI0kpVYR2QNA5PdPohRTjszLQ7_smdoehViayEjJIVEpcdSa_B4OEhH3lNIftz1BRqumzbBJjAK7nJYtpGfvhtyzA" alt=""><figcaption></figcaption></figure>

**Figure 70**

Interestingly, BlackCat creates intermediary files called “checkpoints-\<encrypted file name>” during the encryption process:

<figure><img src="https://lh4.googleusercontent.com/km8qm90yZFJnzUM0lNVw2jR0v_S95QMCrxVUK6AMfP7qxIUOHZTT81RzL9qPCL-KRYAgltiN8G3VYrIppEuQTuVs9lCywIYUcsja9Li18TuNsDb78n0qLETlAgiXATwr05jbtqgNzlJuXfKl6Q" alt=""><figcaption></figcaption></figure>

**Figure 71**

The malware generates 16 random bytes that will be used to derive the AES key:

<figure><img src="https://lh3.googleusercontent.com/mMyNFjGVmqO__cbd62ZNZWgY3VEAAdFURVRkUb1pnukdc2Zj_JOhT2NPIzOdnr9xmSeI9P1U6ZjvHBG4BSmSKE8UudkmL61IVFnfPSNt6ddHnSoPS_Nxl2QMpb-gXRSkY-ieuY_ry1Yc-b2f-w" alt=""><figcaption></figcaption></figure>

**Figure 72**

The ransomware moves the file pointer to the beginning of the file by calling the SetFilePointerEx API (0x0 = **FILE\_BEGIN**):

<figure><img src="https://lh6.googleusercontent.com/bVe83N4VWVpd7yN8sF4JeIsZifsoQQEYnEQzRHtf39fJ_0ShPQPH7v3l7ZonKwzKvhFijiDfqoJKyRmWJMB24I6-6KpvYHMOqsiNrgmOQPCLqg8yuR7--xALFCZ4ZSvn4EQy4obZuLV4pO0N4g" alt=""><figcaption></figcaption></figure>

**Figure 73**

The process reads 4 bytes from the beginning of the file using ReadFile:

<figure><img src="https://lh5.googleusercontent.com/FcMsAHqExnEDJrumvmFZnVXAfv0LOP9hhNqVfmcn_7SYM5PaliZglEbp0FnkXhLX1PCqYzHuvOY7GPkYnO2Zx0XPncDzCyW_YHCoXmeMgxKOw1tKih5o1VRQMC1juIVVjdEgT3_5eDak5Ypy0g" alt=""><figcaption></figcaption></figure>

**Figure 74**

A JSON form containing the encryption cipher (AES), the AES key used to encrypt the file, the data, and the chunk size, is constructed in the process memory:

<figure><img src="https://lh6.googleusercontent.com/ZYjFEOHMYidU6PzhJi1sgvo9cQWBSxJlOePqE4o_I4-oC2S-tRyg4Yw72lJfiLE4K2WJJEZUFPonoYkpaH-1rulAEELXCXXxjyIeEtXh226HEa76c8nva4uNSNqSpBr1m5BeR9cwIHc7PsAH7A" alt=""><figcaption></figcaption></figure>

**Figure 75**

The binary generates 0x50 (80) random bytes that are used to border the JSON form. The resulting buffer has a size of 256 bytes and is rotated using instructions such as pshuflw:

<figure><img src="https://lh5.googleusercontent.com/-jRFqsakavwOjIlf-cWwp8w6KtywNYtteVUUToNqqzf1GyF4rjGr1WjrjI_pKvzw4zQZUX16ziahl56hM2lDCCmTBlJuX8MkWgz3Ag0ah1AsDwRXkhR_uKo1tR7g2tYNCWjCJKnzs5GWBiFrqw" alt=""><figcaption></figcaption></figure>

**Figure 76**

<figure><img src="https://lh3.googleusercontent.com/Hogte8QPgocCXWxA_uZfN6ky78cyVROaa_khnqP4VYPKfMrGssHuHIiXMcJ4ZYZPFFow0OMD0aj9fCXA6YYVjGIQTSzIAjfoD_5Mzuldc2gd4-DQ3KUkzG6v-pAGDTXBHUhEjj2AgHOH_x5A-g" alt=""><figcaption></figcaption></figure>

**Figure 77**

A 4-byte border "19 47 B2 CE" that separates the encrypted file content from the encrypted AES key is written to the file:

<figure><img src="https://lh3.googleusercontent.com/tON0X8LtJq-E85nWEE3DG9FoPgZv0bAelR5oDCWXwCJiRt72diyJo9MgyL5oDakjHEzite4c4r8Ki8Phh7V03L7dho0XZ_sAQoVg1B3FkLyv1Q9nQnP757FcCX1P3m2LZBY9b7AtaTDFu4aB3w" alt=""><figcaption></figcaption></figure>

**Figure 78**

The buffer that contains the AES key presented in figure 77 is encrypted with the RSA public key from the BlackCat configuration. The result is written to the file using WriteFile:

<figure><img src="https://lh6.googleusercontent.com/0rUnhn-VWXbgpPSOn7l4O7EiQGrBGYEYCuRHgk0RuO2RDLwpdMeGVfMSrEzoOJ5kd2yKtsJGgy53YR4wTangw31L4VIBqaif_zVN-VKJLT3Qpm-6HJA-ry_WfVFkBeTDV2lgBNpPeyVALdbWJg" alt=""><figcaption></figcaption></figure>

**Figure 79**

The size of encrypted key (0x100) is written to the file:

<figure><img src="https://lh4.googleusercontent.com/1bAQs8pmJlgUNT91Xawx5-w4MKSzL93aDtDv7rZAdezH3o-PS1fnJEDyYpuurOKXIpVcpbhqqPNvcQA766-BqfDAcP9PjXUKa_n6lukkecaWbZBWCdFBJGQQaoAzmcIynB4Ea_w7Ymt8Ve-Hyw" alt=""><figcaption></figcaption></figure>

**Figure 80**

The file content is read by using the ReadFile function:

<figure><img src="https://lh5.googleusercontent.com/B5PedUznbId4moaznUqvfnZTFP13oxdia2lZD_Gm-9WqWyuF0gGj8COY7v7a8WsY6H8TOQvbieY3bS7OROMic6S3_4Ki6lkRaJH05dN-LaxfjbYU80ZVufb9psslebYxFZO2IkuQ_gwFLi6tQg" alt=""><figcaption></figcaption></figure>

**Figure 81**

The file content is encrypted using the AES-128 algorithm. The malware uses the aesenc and aesenclast instructions for this purpose:

<figure><img src="https://lh3.googleusercontent.com/oxmucG51jYXdZ35xeOgF8u9SIfsLEXQWHigbySgxYnq_m_MPY9klfrZWVeGyZ8bUwG_F-LXEvzgyn1ldiaqvDC_8oZOsOYoXzXhZxIVErs0sO20bPD13g2NMszDY-ywZuIUlDMt4FIBjSzPWvg" alt=""><figcaption></figcaption></figure>

**Figure 82**

<figure><img src="https://lh5.googleusercontent.com/rAItDD6BuTCPKH0EeqH9PrhQJELXlMRRCDa965ReER3h-r18R13kckYykAQxVvVr7Jc3hbclc0R7VBXoetvdDQPM9jnK6_8dMGHWH8FxLC62OJl-8vLyM6rieX3ZKcc2o6gP11p8QCo0dL2Hig" alt=""><figcaption></figcaption></figure>

**Figure 83**

The encrypted file content is written back to the file using WriteFile:

<figure><img src="https://lh4.googleusercontent.com/ZltxBhy1MOKUcGz67m-JcBKgPwOmHKU8blCYjNsF3CC-Coz38oVwT9g2hVAkt5OwX_LcAXev7dTDahk8v_NeexVffKoCNGjmT2J-7d54bwbR16qqzIZgf5CGnzUmpXWupY0WSt2koGLmykCVPw" alt=""><figcaption></figcaption></figure>

**Figure 84**

An example of an encrypted file is displayed below:

<figure><img src="https://lh3.googleusercontent.com/8dqPvNYkkSvJrp9ql3Dt1pXFsBs8bogbGdkIhDElSqk8M4jrCbxGHy0oFy-oCfesjY47nWcZgWVY_tjURcEp1tK-LvF9sADoVU_vE_5-x4xPGg3c3KK61dCAje2QZNvlaz5i1krQhEdfUSx_-w" alt=""><figcaption></figcaption></figure>

**Figure 85**

The ransomware creates a PNG image called “RECOVER-uhwuvzu-FILES.txt.png”:

<figure><img src="https://lh4.googleusercontent.com/4aPDmA2z-ughfr5wgBOuNpzBqmDxEw1TozVvJW4YUWloZGMkDY9PQJfCvS_8sTVOZnaa0mZvsNyn8DavXoBfYJRZsTMiSyb-UK-1fmXuem9Bld3noibTCG98W8ksKk-srC_wJYLqNJDRU48Mxw" alt=""><figcaption></figcaption></figure>

**Figure 86**

<figure><img src="https://lh4.googleusercontent.com/hi8hV0SRGjDdLttgihUNAgoI3hD4ayN5GBAX6GacePKX6SHHRl9YH1mq58Qv5U8CjmNLZCmVQO0SsxE-SwofiZj_uqlUHFQD0Dctm23hh9qlRo4p6JXDsDPKWDaIqN3Rrex4Xje5QwoVbfgdgA" alt=""><figcaption></figcaption></figure>

**Figure 87**

The Desktop wallpaper is changed to the above image by calling the SystemParametersInfoW API (0x14 = **SPI\_SETDESKWALLPAPER**, 0x3 = **SPIF\_UPDATEINIFILE** | **SPIF\_SENDCHANGE**):

<figure><img src="https://lh5.googleusercontent.com/c2XwRN52Cs0TjvRGk6mNPevStN0QQ7MqivZrUykTHVOlIJ5ObJMlWrpVclgq2xm-K4Jx356xbE-KCwlfMuzUWz9kNHiL_hVl0_yyi61A2RcWc1HxsEUvQPfb7RW6MUoAF9eglOedTR6OE-KO_w" alt=""><figcaption></figcaption></figure>

**Figure 88**

## **Running with the --verbose parameter**

The ransomware writes multiple actions to the command line output:

<figure><img src="https://lh5.googleusercontent.com/7JuTlgTS9ZBIPySf6w2Lvo7EJwFqVRUfKxQN5f8tWKlba8whikqQWKYjkysSHKNI8LCxVwHF0javrj_j7nOMkr5vWtyhDpexJcuRyuInbICU_QTh3hWsEP-cB6K7YC6OXjYeZnRl5jzWaZm6JA" alt=""><figcaption></figcaption></figure>

**Figure 89**

## **Running with the --extra-verbose --ui parameters**

The malware presents the relevant information in the following window:

<figure><img src="https://lh3.googleusercontent.com/esCMaQoOnE7g0nVSBvR-asxkQQzBGu3L5_Rw2ai6nOwUZE8tXzMGhNfG0xrxf9rJVI8XpPGrCfMLUNDAosA3anH23qfoQntzyq5nf_SHn2WHadSZ2y__-XFKf0SW0mMv5fjlH1n2dkYmO7wUwA" alt=""><figcaption></figcaption></figure>

**Figure 90**

\


## **Indicators of Compromise**

**Pipe**

\\\\.\pipe\\\_\_rust\_anonymous\_pipe1\_\_.\<Process ID>.\<Random number>

**BlackCat Ransom Note**

RECOVER-uhwuvzu-FILES.txt

**Files created**

checkpoints-\<Filename>.uhwuvzu

RECOVER-uhwuvzu-FILES.txt.png

**Processes spawned**

cmd.exe /c "wmic csproduct get UUID"

cmd.exe /c "fsutil behavior set SymlinkEvaluation R2L:1”

cmd.exe /c “fsutil behavior set SymlinkEvaluation R2R:1”

cmd.exe /c “iisreset.exe /stop”

cmd.exe /c “vssadmin.exe Delete Shadows /all /quiet”

cmd.exe /c “wmic.exe Shadowcopy Delete”

cmd.exe /c “bcdedit /set {default}”

cmd.exe /c “bcdedit /set {default} recoveryenabled No”

cmd.exe /c for /F "tokens=\*" %1 in ('wevtutil.exe el') DO wevtutil.exe cl %1

cmd.exe /c “reg add HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters /v MaxMpxCt /d 65535 /t REG\_DWORD /f”

cmd.exe /c “arp -a”
