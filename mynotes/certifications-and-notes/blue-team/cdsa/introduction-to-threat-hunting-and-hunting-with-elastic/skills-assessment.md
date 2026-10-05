> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/introduction-to-threat-hunting-and-hunting-with-elastic/skills-assessment.md).

# Skills Assessment

`Hunt 1`: Create a KQL query to hunt for ["Lateral Tool Transfer"](https://attack.mitre.org/techniques/T1570/) to `C:\Users\Public`. Enter the content of the `user.name` field in the document that is related to a transferred tool that starts with "r" as your answer.

```
event.code : 11 and file.directory : "C:\Users\Public"
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fsm3uh3PbShjxfJ6IjRug%2Fimage.png?alt=media&amp;token=4f5fb8af-951d-4d8a-94c0-d53dde3e6ac3" alt=""><figcaption></figcaption></figure>

`Hunt 2`: Create a KQL query to hunt for ["Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder"](https://attack.mitre.org/techniques/T1547/001/). Enter the content of the `registry.value` field in the document that is related to the first registry-based persistence action as your answer.

```
event.code : "13"
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FpA4hZdknQsYHtH4itU62%2Fimage.png?alt=media&amp;token=a9ad8d2b-c449-4dfc-be83-66b6759c61c0" alt=""><figcaption></figcaption></figure>

`Hunt 3`: Create a KQL query to hunt for ["PowerShell Remoting for Lateral Movement"](https://www.ired.team/offensive-security/lateral-movement/t1028-winrm-for-lateral-movement). Enter the content of the `winlog.user.name` field in the document that is related to PowerShell remoting-based lateral movement towards DC1.

```
event.code:"4104" and powershell.file.script_block_text : "DC1"
```

* `powershell.file.script_block_text` refers to a field in logging or security analysis contexts, particularly within the Elastic Stack (Elasticsearch, Kibana, Winlogbeat) or similar security information and event management (SIEM) systems. This field contains the actual content of a PowerShell script block that was executed on a system.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F8KLiikElEVfhjSUHUIcz%2Fimage.png?alt=media&amp;token=3ed33281-b975-4270-b8a7-39bf03c663df" alt=""><figcaption></figcaption></figure>
