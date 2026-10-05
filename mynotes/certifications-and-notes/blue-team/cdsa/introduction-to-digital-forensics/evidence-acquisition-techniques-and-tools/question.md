> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/introduction-to-digital-forensics/evidence-acquisition-techniques-and-tools/question.md).

# Question

Visit the URL "<https://127.0.0.1:8889/app/index.html#/search/all>" and log in using the credentials: admin/password. After logging in, click on the circular symbol adjacent to "Client ID". Subsequently, select the displayed "Client ID" and click on "Collected". Initiate a new collection and gather artifacts labeled as "Windows.KapeFiles.Targets" using the \_SANS\_Triage configuration. Lastly, examine the collected artifacts and enter the name of the scheduled task that begins with 'A' and concludes with 'g' as your answer.

* I don't know how to solve this using the velociraptor.
* I used PowerShell.

```
Get-ScheduledTask | Where-Object {$_.TaskName -like “A*g”} 
```
