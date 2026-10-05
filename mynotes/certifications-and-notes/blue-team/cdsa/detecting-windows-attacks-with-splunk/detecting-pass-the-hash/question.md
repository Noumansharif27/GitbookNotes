> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/detecting-windows-attacks-with-splunk/detecting-pass-the-hash/question.md).

# Question

A Pass-the-Hash attack took place during the following timeframe earliest=1690543380 latest=1690545180. Enter the involved ComputerName as your answer.

```
index=main earliest=1690543380 latest=1690545180 source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo
| table _time, ComputerName, EventCode, user, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
```
