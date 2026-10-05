> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/intrusion-detection-with-splunk/question.md).

# Question

#### Question 1

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an SPL search against all data the other process that dumped lsass. Enter its name as your answer. Answer format: \_.exe

```
index="main" EventCode=10 lsass | stats count by SourceImage, RuleName
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FOSay78Tki04rj0dpWVT2%2Fimage.png?alt=media&amp;token=69856663-c8d1-4b82-94ba-34af654a2d68" alt=""><figcaption></figcaption></figure>

* **Description**: `rundll32.exe` was used to load a DLL/loader that accessed lsass.exe memory.
* **Likely Tool/Method**: Using tools like Mimikatz (or equivalent functionality) to extract hashes and credentials.
* **Action taken**: Read/dump the memory of the lsass.exe process.
* **Objective**: Credential theft (hashes, clear text passwords, Kerberos tickets).
* **MITER ATT\&CK Technique**: Credential Dumping (T1003.001 – LSASS Memory)

***

#### Question 2

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the method through which the other process dumped lsass. Enter the misused DLL's name as your answer. Answer format: \_.dll

```
index="main" "rundll32.exe" *.dll
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsrucqQzbigED9gHudExf%2Fimage.png?alt=media&amp;token=b498f053-efd6-4e20-a120-afe507edd805" alt=""><figcaption></figcaption></figure>

* **Responsible process**: rundll32.exe
* **Misused DLL**: comsvcs.dll
* **Action taken**: Dump the lsass.exe process
* **Objective**: Theft of credentials stored in system memory

***

#### Question 3

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an SPL search against all data any suspicious loads of clr.dll that could indicate a C# injection/execute-assembly attack. Then, again through SPL searches, find if any of the suspicious processes that were returned in the first place were used to temporarily execute code. Enter its name as your answer. Answer format: \_.exe

```
index="main" CallTrace="*UNKNOWN*" SourceImage!="*Microsoft.NET*" CallTrace!=*ni.dll* CallTrace!=*clr.dll* CallTrace!=*wow64* | where SourceImage!=TargetImage | stats count by SourceImage
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FaKEA8kCjJEYnBeJIkPhC%2Fimage.png?alt=media&amp;token=7cb25b4b-5520-4444-8ef8-08b6db0b09ce" alt=""><figcaption></figcaption></figure>

* **Responsible process**: rundll32.exe&#x20;
* **DLL/Library observed**: clr.dll
* **Description**: rundll32.exe loaded clr.dll, suggesting code injection via .NET runtime.
* **Probable method**: loading malicious .NET assemblies into memory (injection/execution without writing to disk).
* **Action taken**: Execution of malicious code within the rundll32.exe process.
* **Goal**: stealth execution of payloads, evasion of detection and persistence/remote control.
* **MITER ATT\&CK Technique**: Process Injection (T1055) / Signed Binary Proxy Execution / Living off the Land (T1218).

***

#### Question 4

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the two IP addresses of the C2 callback server. Answer format: 10.0.0.1XX and 10.0.0.XX

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=3  | stats count by RuleName, DestinationIp
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FISDA5zmnMMJ9pYYrUM3J%2Fimage.png?alt=media&amp;token=b6d65289-4d1d-49f2-bac4-64f74d8ac15c" alt=""><figcaption></figcaption></figure>

* **Tactic**: Defense Evasion (TA0005) — T1218 (Signed/System Binary Proxy Execution) is a defense evasion technique according to MITER.
* **What the technique does**: Adversaries use signed or trusted binaries (LOLBins, e.g.: rundll32.exe, regsvr32.exe, mshta.exe, notepad.exe) to proxy‑execute malicious code and bypass signature/enforcement controls.

***

#### Question 5

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the port that one of the two C2 callback server IPs used to connect to one of the compromised machines. Enter it as your answer.

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=3 (SourceIp="10.0.0.186" OR SourceIp="10.0.0.91")
| stats values(DestinationPort) as destination_ports
```
