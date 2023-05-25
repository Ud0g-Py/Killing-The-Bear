---
description: '2023-05-25'
---

# New tricks of APT29 – update on the CERT.PL report

A new sample has been uploaded to VirusTotal, and its characteristics strongly resemble QUARTERRIG, a malware recently [analyzed by CERT.PL](https://www.gov.pl/web/baza-wiedzy/espionage-campaign-linked-to-russian-intelligence-services) and linked to APT29. In said analysis, the campaign was named “**Note**“. Based on the aforementioned report, the purpose of this post is to show the new features of this new campaign that we named “**Information**“.

The hash of the sample made public in VirusTotal is b422ba73f389ae5ef9411cf4484c840c7c82f2731c6324db0b24b6f87ce8477d, and only 3 antivirus engines target the sample as malicious during the writing of this post.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/f50-1024x448.png" alt="" height="448" width="1024"><figcaption><p>Sample in VirusTotal analysed in this report</p></figcaption></figure>

APT29 is a hacker group allegedly affiliated with **one or more Russian intelligence agencies**. It is a sophisticated group that has been carrying out attacks against European governments and diplomatic agencies since 2008.

The main entry vector for APT29 is email. Using this input vector, attackers attach a PDF with a link that will download an ISO.

## New campaign: “Information”

This new campaign, which will be referred to as **Information**, contains a structure very similar to the **Note** campaigns shown in the CERT.PL report. The samples analyzed in that report are from March. However, from Lab52, **we have observed a change in the operation of this type of malware since April, and in the latest analyzed samples, the injection method has varied**. In this post, we use one of the latest samples to highlight the new changes in the mechanisms employed.

This time **the file containing the shellcode is located in a file called “dbg.info”** unlike what we have been observing in previous campaings. The **Information.iso** contains:

* AppvlsSubsystems64.dll – DLL used to load a legitime system DLL and inject the shellcode into it.
* dbg.info – shellcode.
* Information .exe – Legitime binary **signed by Microsoft**. This will be use to load AppvlsSubsystems64.dll (by DLL Side-Load).

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-14.png" alt="" height="195" width="864"><figcaption><p>Contents of “Information.iso” versus “Note.iso” (past campaign)</p></figcaption></figure>

The compilation date for **AppvIsvSubsystems64.dll** in this new campaign is more recent than the previous one. This could suggest that changes were made to improve the sample.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-15.png" alt="" height="424" width="709"><figcaption><p>Compilation date AppvIsvSubsystems64.dll – Information</p></figcaption></figure>

This post focuses on the main diferences between the previous campaing and the new one, in order to contribute to the community. The most noticeable change is the injection technique. Nevertheless some addional notes are added for the curious.

The objective is the same: the executable (Information.exe in this case) will be used to execute two DLLs. The first DLL is AppvlsvSubsystems64.dll, that will be loaded by the process as part of its execution. The second DLL will be loaded by AppvlsvSubsystem64.dll. **In this case, however, the second DLL will be carefully modified with different techniques in order to minimize the detection methods.**

Therefore, Information.exe, that is a legitime binary, will be the container for the malware to be executed. In order to do that, it will load AppvIsvSubsystems64.dll who needs to identify a suitable system DLL to be modified – with the shellcode – before the load in Information.exe. **The main changes are located in AppvIsvSubsystems64.dll**. Also, the command and control (C2) varies.

The following diagram describes the infection chain analysed in this post.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/infection.png" alt="" height="352" width="733"><figcaption><p>Infection chain – Information campaign</p></figcaption></figure>

## Description of the APIs

When running the sample, the first difference lies in how the **AppvIsvSubsystems64.dll** loads the functions needed for the execution. In the “Note” campaing, the funcions are loaded at an early stage than the Information campaign. Moreover, the decription in this last case occurs at a different point, later in during the execution.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-16-1024x331.png" alt="" height="331" width="1024"><figcaption><p>Entry point of AppvIsvSubsystems64.dll – Information vs Note</p></figcaption></figure>

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-17-1024x381.png" alt="" height="381" width="1024"><figcaption><p><em>Differences in .data – Informtion vs Note</em></p></figcaption></figure>

## Shellcode injection

The way in which the sample injects shellcode into memory also varies from campaign to campaign. As can be seen in the report, “Note” reserves a memory space in its process and writes the shellcode into it.

In this case the injection process is more sophisticated, **the injection will be triggered by modifying the .text section of legitimate libraries**. The following actions are performed by **AppvIsvSubsystems64.dll**.

In order to do that, first it goes through each of the DLLs stored in **System32**.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-21.png" alt="" height="306" width="556"><figcaption></figcaption></figure>

Next check the size of the DLL, it will only be a candidate if it has a size greater than or equal to **782629 bytes** (Shellcode size).

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-22.png" alt="" height="565" width="936"><figcaption><p>Test DLL size</p></figcaption></figure>

The sample also checks that the candidate DLL is not already loaded in the executable (Information.exe). That is, the following list are the DLLs already loaded in Information.exe, and, therefore, discarded by the malware:

| msvcp140.dll           | combase.dll  |
| ---------------------- | ------------ |
| msi.dll                | sechost.dll  |
| appvisvsubsystem64.dll | msvcrt.dll   |
| vcruntime140.dll       | kernel32.dll |
| vcruntime140\_1.dll    | imm32.dll    |
| ucrtbase.dll           | gdi32.dll    |
| bcrypt.dll             | rpcrt4.dll   |
| win32u.dll             | shell32.dll  |
| msvcp\_win.dll         | advapi32.dll |
| kernelbase.dll         | user32.dll   |
| gdi32full.dll          | ntdll.dll    |

After said checks, the malware has a set of candidate DLLs. The DLL selected for injection will vary in each execution, thanks to a randomization function implemented in the sample. **A list of candidate DLLs by default – those that satisfy the requirements needed by the malware – in Windows 10 64b is provided at the end of this post**. This may vary depending on the operating system. However, following the steps in this post is not difficult to prepare your own script. A similar technique is described in the [Netero1010 post](https://www.netero1010-securitylab.com/evasion/alternative-process-injection).

The malware selects the DLL used for the injection as follows.

First, it uses the system time as a seed to apply a series of arithmetic operations on it. The result will be the seed of the next DLL to be checked.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-23.png" alt="" height="267" width="526"><figcaption><p>Seed generation using system time</p></figcaption></figure>

Interestingly, the values it uses for multiplication and addition are identical to those provided in a [“Holiday Hack Challenge 2019”](https://www.holidayhackchallenge.com/2019/winners/HolidayHackChallenge2019Report\_Michael-Pella.pdf) challenge organized by SANS. In particular, in the challenge there is a function called “**super\_secure\_random**” that performs the same operations with the same operands to a seed (“state”).

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-24.png" alt="" height="257" width="604"><figcaption><p>Comparison of the CTF algorithm (left) with the sample (right)</p></figcaption></figure>

Finally, it checks if this resulting number is a multiple of 25 and if it is, it continues with the injection.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-25.png" alt="" height="91" width="371"><figcaption><p>Choice of random DLL</p></figcaption></figure>

Once the DLL has been chosen, the sample accesses the library with CreateFileW (in the execution used to take the pictures for this example the DLL selected was “AppContracts.dll”).

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-26.png" alt="" height="498" width="811"><figcaption><p>DLL opened</p></figcaption></figure>

At this point, the sample reads the headers of the libraries to pass a round of checks again. First, look at the “Magic” value to verify that it is a 64-bit system DLL (PE64).

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-27.png" alt="" height="191" width="851"><figcaption><p>Magic value of the DLL candidate</p></figcaption></figure>

Another comprobation is to **check the “SizeOfImage”** field, which refers to the **size to reserve in memory to load the executable**. If it is less than **782629** **bytes** (shellcode size), it is discarded.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-28.png" alt="" height="251" width="891"><figcaption><p>SizeOfCode of the candidate DLL</p></figcaption></figure>

Finally, the sample checks the size of the “.text” section (where the shellcode will be injected) and verifies that it is larger than **782629** **bytes**.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-29.png" alt="" height="191" width="845"><figcaption><p>Size of “.text” section of the candidate DLL</p></figcaption></figure>

Unfortunately, at this point AppContracts.dll (the DLL selected in this execution) does not meet the injection requirements, and the search for the next optimal DLL is **AppxPackaging.dll**, in this example.

After this screening, **the malware will have about 283 candidates for injection** in the system used. At the end of the post you will find a table with the possible DLLs in which the malware could be injected, **consiering the size required for the injection by this sample (782629**) **and the system where it be executed**.

At this point, the chosen DLL (in this example, at this moment, AppxPackaging.dll) is ready to receive the shellcode, using the following injection method.

**Step1.** Subsequently, it makes a call to **NtCreateSection** to create a memory section in the process.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-30-1024x244.png" alt="" height="244" width="1024"><figcaption><p>Create section to map the DLL</p></figcaption></figure>

**Step2.** And map the library in that section with **ZwMapViewOfSection.**

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-32-1024x272.png" alt="" height="272" width="1024"><figcaption><p>Map the DLL</p></figcaption></figure>

**Step3.** Once the library is mapped in memory, the write permissions are modified to be able to write the shellcode in it. **The address pointed to by VirtualProtect corresponds to the “.text” section of the DLL**.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-33-1024x213.png" alt="" height="213" width="1024"><figcaption><p>Change protection to RW (.text section)</p></figcaption></figure>

**Step4.** After that, write the shellcode in the “.text” section.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-34.png" alt="" height="573" width="937"><figcaption><p>Write the shellcode in the .text section. Left: not infected. Right: infected</p></figcaption></figure>

**Step5**. Finally the malware changes the permissions of the “.text” section back to “EXECUTABLE\_READ” again.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/image-35-1024x238.png" alt="" height="238" width="1024"><figcaption><p>Change the .text section back to RX</p></figcaption></figure>

Once the shellcode is loaded, the execution is identical to what can be seen in the QUARTERRIG report of CERT.PL, but with a different C2. In this case, the actors use the URL **hxxps:]]//pizzais.com/order.php**.

## Timeline

As mentioned before, **Lab52 has observed the evolution in techniques since April**. Specifically, here we provide an additional hash of a different file published on VirusTotal that uses this injection technique, found during the writing of this post.

| Hash                                                             | Description                                                      | Date       |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- | ---------- |
| C71EC48A59631BFA3F33383C1F25719E95E5A80936D913AB3BFE2FEB172C1C5E | Notes.iso injecting the shellcode in the .txt section of the DLL | 28/04/2023 |

Therefore, at the least in a previous registered case, the .iso file still bears the name “Note.iso,” which could indicate that **the new technique was already in use** in some samples of the previous campaign. However, the most recent observations show a change in their name to “Information”.

<figure><img src="https://lab52.io/blog/wp-content/uploads/2023/05/timeline.png" alt="" height="182" width="481"><figcaption><p>Short timeline – Changes in the injection technique</p></figcaption></figure>

## Conclusions

Just as QUARTERRIG was the evolution of HALFRIG, in this new campaign APT29 has modified the logic of its dll loader “Applvsubsystem64.dll” to make it more sophisticated.

The fact of using legitimate random DLLs for injection instead of the process memory itself, adds another layer of complexity to the way the shellcode is loaded.

A list of system candidates to be injected is provided in this post. However, it must be considered carefully because these are extracted of a specific system, following the previous steps. The analysts can follow the steps mentioned here in order to build their own script to get the list of DLL candidates to be used by the authors this new campaing.

## Indicators of Compromise (IOC)

| <p><br>File</p>         | Hash                                                             |
| ----------------------- | ---------------------------------------------------------------- |
| Information.iso         | B422BA73F389AE5EF9411CF4484C840C7C82F2731C6324DB0B24B6F87CE8477D |
| Information .exe        | 6C55195F025FB895F9D0EC3EDBF58BC0AA46C43EEB246CFB88EEF1AE051171B3 |
| AppvIsvSubsystems64.dll | E7C49758BAE63C83D251CACBFADA7C09AF0C3038E8FF755C4C04F916385805D8 |
| dbg.info                | 5F6219ADE8E0577545B9F13AFD28F6D6E991326F3C427D671D1C1765164B0D57 |

| C2                              |
| ------------------------------- |
| hxxps:]]//pizzais.com/order.php |

| Filesystem                                   | Description                                         |
| -------------------------------------------- | --------------------------------------------------- |
| C:\Users\user\AppData\Local\MSOfficeUpdate\\ | AppvIsvSubsystems64.dll, Information .exe, dbg.info |

| Persistence                                                       | Value                                                       |
| ----------------------------------------------------------------- | ----------------------------------------------------------- |
| HKEY\_CURRENT\_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run | C:\Users\user\AppData\Local\MSOfficeUpdate\Information .exe |

## Candidate system DLLs for injection

_The following DLLs will pass the checks for this sample (size and other requirements). This list may vary on the target system depending on the version and the system software installed. Please check._

| aadtb.dll                              | ActiveSyncProvider.dll                    | adtschema.dll                                            |
| -------------------------------------- | ----------------------------------------- | -------------------------------------------------------- |
| APMon.dll                              | appraiser.dll                             | AppXDeploymentExtensions.desktop.dll                     |
| AppXDeploymentExtensions.onecore.dll   | AppXDeploymentServer.dll                  | AppxPackaging.dll                                        |
| AudioEng.dll                           | AudioSes.dll                              | audiosrv.dll                                             |
| AuthFWSnapin.dll                       | AuthFWSnapIn.Resources.dll                | AzureSettingSyncProvider.dll                             |
| bcastdvruserservice.dll                | BingMaps.dll                              | cavo2gui.dll                                             |
| cdp.dll                                | cdprt.dll                                 | CertEnroll.dll                                           |
| Chakra.dll                             | cimwin32.dll                              | ClipSVC.dll                                              |
| CloudExperienceHostCommon.dll          | cmiv2.dll                                 | comsvcs.dll                                              |
| ConstraintIndex.Search.dll             | ContentDeliveryManager.Utilities.dll      | CoreShell.dll                                            |
| CoreUIComponents.dll                   | crypt32.dll                               | d2d1.dll                                                 |
| d3d10.dll                              | d3d10warp.dll                             | d3d11.dll                                                |
| D3D12Core.dll                          | d3d9.dll                                  | D3DCompiler\_47.dll                                      |
| dbgeng.dll                             | dbghelp.dll                               | dcomp.dll                                                |
| DeviceFlows.DataModel.dll              | dfshim.dll                                | diagperf.dll                                             |
| diagtrack.dll                          | directml.dll                              | DMWmiBridgeProv.dll                                      |
| dosvc.dll                              | drvstore.dll                              | dui70.dll                                                |
| dwmcore.dll                            | dwmscene.dll                              | DWrite.dll                                               |
| dxilconv.dll                           | edgeangle.dll                             | EdgeContent.dll                                          |
| edgehtml.dll                           | efscore.dll                               | EmailApis.dll                                            |
| enterprisecsps.dll                     | esent.dll                                 | ExplorerFrame.dll                                        |
| FaceProcessor.dll                      | FaceRecognitionEngineAdapter.dll          | fhuxpresentation.dll                                     |
| FluencyDS.dll                          | FntCache.dll                              | FrameServer.dll                                          |
| GdiPlus.dll                            | gpsvc.dll                                 | HologramWorld.dll                                        |
| Hydrogen.dll                           | icu.dll                                   | ieframe.dll                                              |
| inetcomm.dll                           | InputHost.dll                             | InputService.dll                                         |
| InstallService.dll                     | IntelWifiIhv08.dll                        | ISM.dll                                                  |
| jscript9.dll                           | kerberos.dll                              | KernelBase.dll                                           |
| libcrypto.dll                          | localspl.dll                              | LocationFramework.dll                                    |
| lpasvc.dll                             | lsasrv.dll                                | MapGeocoder.dll                                          |
| MapRouter.dll                          | MCRecvSrc.dll                             | MdmDiagnostics.dll                                       |
| MemoryAnalyzer.dll                     | MessagingDataModel2.dll                   | mfasfsrcsnk.dll                                          |
| mfc140.dll                             | mfc140u.dll                               | mfc42.dll                                                |
| mfc42u.dll                             | mfcore.dll                                | MFMediaEngine.dll                                        |
| mfmkvsrcsnk.dll                        | mfmp4srcsnk.dll                           | mfmpeg2srcsnk.dll                                        |
| mfnetcore.dll                          | mfnetsrc.dll                              | mfperfhelper.dll                                         |
| mfplat.dll                             | mfreadwrite.dll                           | mfsrcsnk.dll                                             |
| mfsvr.dll                              | Microsoft.Bluetooth.Service.dll           | Microsoft.Graphics.Display.DisplayEnhancementService.dll |
| migcore.dll                            | MiracastReceiver.dll                      | mispace.dll                                              |
| mmcndmgr.dll                           | mmgaclient.dll                            | MSAJApi.dll                                              |
| msctf.dll                              | msdtctm.dll                               | msftedit.dll                                             |
| mshtml.dll                             | msmpeg2vdec.dll                           | MSPhotography.dll                                        |
| mssrch.dll                             | mstscax.dll                               | MSTTSEngine\_OneCore.dll                                 |
| MSVidCtl.dll                           | msvproc.dll                               | msxml3.dll                                               |
| msxml6.dll                             | MSxpsPCL6.dll                             | MSxpsPS.dll                                              |
| mxdwdrv.dll                            | NetworkMobileSettings.dll                 | NotificationController.dll                               |
| ole32.dll                              | OpcServices.dll                           | opengl32.dll                                             |
| PCPKsp.dll                             | perf\_nt.dll                              | pidgenx.dll                                              |
| pla.dll                                | PresentationNative\_v0300.dll             | PrintConfig.dll                                          |
| PrintConfig.dll                        | PrintConfig.dll                           | PrintConfig.dll                                          |
| qmgr.dll                               | quartz.dll                                | rasapi32.dll                                             |
| rasmans.dll                            | rdpbase.dll                               | rdpcore.dll                                              |
| rdpcorets.dll                          | rdpnano.dll                               | rdpserverbase.dll                                        |
| rdpsharercom.dll                       | reseteng.dll                              | ResetEngine.dll                                          |
| RP2DSN32.dll                           | rpcss.dll                                 | rtmcodecs.dll                                            |
| rtmpal.dll                             | rtmpltfm.dll                              | sapi.dll                                                 |
| sapi\_onecore.dll                      | sbe.dll                                   | sdengin2.dll                                             |
| SettingsHandlers\_nt.dll               | setupapi.dll                              | SpeechPal.dll                                            |
| sppobjs.dll                            | spsreng.dll                               | spsreng\_onecore.dll                                     |
| spwizimg.dll                           | SRH.dll                                   | StartTileData.dll                                        |
| storagewmi.dll                         | sysmain.dll                               | SystemSettings.Handlers.dll                              |
| SystemSettingsThresholdAdminFlowUI.dll | TaskFlowDataEngine.dll                    | termsrv.dll                                              |
| TextInputMethodFormatter.dll           | TokenBroker.dll                           | TpmCoreProvisioning.dll                                  |
| tquery.dll                             | tsf3gip.dll                               | twinapi.appcore.dll                                      |
| twinui.dll                             | twinui.pcshell.dll                        | uDWM.dll                                                 |
| UIAutomationCore.dll                   | UIRibbon.dll                              | UIRibbonRes.dll                                          |
| Unistore.dll                           | UpdateAgent.dll                           | urlmon.dll                                               |
| usbmon.dll                             | UserDataService.dll                       | usermgr.dll                                              |
| VBoxDispD3D.dll                        | VBoxDispD3D.dll                           | VBoxGL.dll                                               |
| VBoxGL.dll                             | VBoxGL-x86.dll                            | VBoxMRXNP.dll                                            |
| VBoxNine.dll                           | VBoxNine.dll                              | VBoxSVGA.dll                                             |
| VBoxSVGA.dll                           | VBoxSVGA-x86.dll                          | vo28gui.dll                                              |
| vssapi.dll                             | wbemcore.dll                              | webplatstorageserver.dll                                 |
| WebRuntimeManager.dll                  | webservices.dll                           | wevtsvc.dll                                              |
| win32spl.dll                           | WindowManagement.dll                      | Windows.AI.MachineLearning.dll                           |
| Windows.ApplicationModel.Store.dll     | Windows.CloudStore.dll                    | WindowsCodecs.dll                                        |
| WindowsCodecsRaw.dll                   | Windows.Data.Pdf.dll                      | Windows.Devices.Bluetooth.dll                            |
| Windows.Devices.Perception.dll         | Windows.Devices.PointOfService.dll        | Windows.Globalization.dll                                |
| Windows.Graphics.Printing.3D.dll       | Windows.Graphics.Printing.Workflow.dll    | Windows.Internal.Signals.dll                             |
| WindowsInternal.Xaml.Controls.Tabs.dll | Windows.Media.dll                         | Windows.Media.Editing.dll                                |
| Windows.Media.Protection.PlayReady.dll | Windows.Media.Speech.dll                  | Windows.Media.Streaming.dll                              |
| Windows.Mirage.dll                     | Windows.Networking.BackgroundTransfer.dll | Windows.Security.Authentication.Web.Core.dll             |
| Windows.StateRepository.dll            | windows.storage.dll                       | windowsudk.shellcommon.dll                               |
| Windows.UI.Cred.dll                    | Windows.UI.Immersive.dll                  | Windows.UI.Input.Inking.Analysis.dll                     |
| Windows.UI.Input.Inking.dll            | Windows.UI.Logon.dll                      | Windows.UI.Shell.Internal.AdaptiveCards.dll              |
| Windows.UI.Xaml.Controls.dll           | Windows.UI.Xaml.dll                       | Windows.UI.Xaml.Maps.dll                                 |
| Windows.UI.Xaml.Phone.dll              | Windows.Web.Http.dll                      | winhttp.dll                                              |
| wininet.dll                            | winmde.dll                                | winmsipc.dll                                             |
| winsetup.dll                           | wlansvc.dll                               | wlidsvc.dll                                              |
| WMNetMgr.dll                           | wmp.dll                                   | workfolderssvc.dll                                       |
| WpcDesktopMonSvc.dll                   | Wpc.dll                                   | wpncore.dll                                              |
| WsmSvc.dll                             | wsp\_fs.dll                               | wsp\_health.dll                                          |
| wuaueng.dll                            | wwansvc.dll                               | XblAuthManager.dll                                       |
| XblGameSave.dll                        | XboxNetApiSvc.dll                         | XpsPrint.dll                                             |
| xpsservices.dll                        |                                           |                                                          |



[https://lab52.io/blog/2162-2/](https://lab52.io/blog/2162-2/)
