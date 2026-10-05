> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/detecting-attacker-behavior-with-ttps/question.md).

# Question

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the password utilized during the PsExec activity. Enter it as your answer.

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 CommandLine="*PsExec*"
| rex field=CommandLine "(?i)(?:-p[:=]?\s*['\"]?(?<psexec_password>[^'\"\s]+)['\"]?)|(?:-password[:=]?\s*['\"]?(?<psexec_password2>[^'\"\s]+)['\"]?)"
| eval password=coalesce(psexec_password, psexec_password2)
| where isnotnull(password)
| table _time, Host, User, CommandLine, password
| dedup password
| sort 0 _time
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FbO8e3fMgHh52vE7wgZ2l%2Fimage.png?alt=media&amp;token=acc7ac18-6336-41ac-bd03-5bf46221f7ed" alt=""><figcaption></figcaption></figure>

* **Description**: PsExec was executed to establish remote execution on another host, typically by creating a temporary service and running a remote payload.
* **Objective**: Lateral movement and remote execution—gain control of additional hosts, execute post-exploitation tools, and potentially escalate privileges using compromised credentials.
* **MITRE ATT\&CK Technique**: Remote Services—Windows Admin Shares / SMB / PsExec (T1021.006) (often combined with Valid Accounts (T1078) and Lateral Movement).
