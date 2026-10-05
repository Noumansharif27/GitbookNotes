> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/windows-event-logs-and-finding-evil/skills-assessment.md).

# Skills Assessment

#### Overview

To keep you sharp, your SOC manager has assigned you the task of analyzing older attack logs and providing answers to specific questions.

#### Question 1

By examining the logs located in the "C:\Logs\DLLHijack" directory, determine the process responsible for executing a DLL hijacking attack. Enter the process name as your answer. Answer format: \_.exe

* To detect a DLL hijacking, we need to focus on `Event Type 7` which corresponds to module loading events.
* After loading the log file into the "Event Viewer" we apply a filter for `Event Type 7`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FObDhs0FHYDQqvV1vvd5n%2Fimage.png?alt=media&amp;token=1490993a-a137-4ae8-b67c-56befb3ab73e" alt=""><figcaption></figcaption></figure>

***

#### Question 2

By examining the logs located in the "C:\Logs\PowershellExec" directory, determine the process that executed unmanaged PowerShell code. Enter the process name as your answer. Answer format: \_.exe<br>

* Managed code is not executed directly as an assembly; instead, it is compiled into a bytecode format that the runtime processes and executes. Consequently, a managed process relies on the CLR to execute C# code.
* We filter for type 7 events and search for `clr.dll`, which likely returns the managed process.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FeXmv6XyLLP8w1Fq2EkVA%2Fimage.png?alt=media&amp;token=fe94ac8a-2c53-4565-b891-ac79456af003" alt=""><figcaption></figcaption></figure>

***

#### Question 3

By examining the logs located in the "C:\Logs\PowershellExec" directory, determine the process that injected into the process that executed unmanaged PowerShell code. Enter the process name as your answer. Answer format: \_.exe

```powershell
Get-WinEvent -Path 'C:\Logs\PowershellExec\*' | Where-Object{$_.ID -like "8"} | Where-Object{$_.Message -like "*Calculator.exe*"} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FD6ivSx0Xn72WF5zGU2EZ%2Fimage.png?alt=media&amp;token=01154c8f-6c4a-44f9-9fe4-289f3d33c94f" alt=""><figcaption></figcaption></figure>

* We can search for event ID 8 (Powershell Events)

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F67U3wuo4zJSqiScsuoJC%2Fimage.png?alt=media&amp;token=28294307-d654-4fd2-bf01-f34880e4351f" alt=""><figcaption></figcaption></figure>

#### Question 4

By examining the logs located in the "C:\Logs\Dump" directory, determine the process that performed an LSASS dump. Enter the process name as your answer. Answer format: \_.exe

* To detect this activity, we can rely on a different Sysmon event. Instead of focusing on DLL loads, we shift our attention to process access events. By checking for Sysmon event ID 10, which represents "ProcessAccess" events, we can identify any suspicious attempts to access LSASS.
* I filtered for events with [ID](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon#events) 10 (ProcessAccess) and searched for entries targeting lsass.exe. While there were several results, only one was launched by a suspicious executable file.

```powershell
Get-WinEvent -Path 'C:\Logs\Dump\*' | Where-Object{$_.ID -like "10"} | Where-Object{$_.Message -like "*TargetImage*lsass.exe*"} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fewa5oAPcdV0HhIBTEnTC%2Fimage.png?alt=media&amp;token=0750a2a5-fcb7-4dd4-86a2-ba01b0cba7c4" alt=""><figcaption></figcaption></figure>

***

#### Question 5

By examining the logs located in the "C:\Logs\Dump" directory, determine if an ill-intended login took place after the LSASS dump. Answer format: Yes or No

```
no
```

***

#### Question 6

By examining the logs located in the "C:\Logs\StrangePPID" directory, determine a process that was used to temporarily execute code based on a strange parent-child relationship. Enter the process name as your answer. Answer format: \_.exe

* Finally, I filtered by events with [ID](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon#events) 1 (Process Creation). There were limited results, and only one of them looked suspicious.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FTGWUHHUjAzPlKq7RIYMC%2Fimage.png?alt=media&amp;token=d3f785d5-d4b1-42dd-ab89-fb9ed9fc1965" alt=""><figcaption></figcaption></figure>
