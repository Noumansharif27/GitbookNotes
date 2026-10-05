> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/detecting-windows-attacks-with-splunk/detecting-common-user-domain-recon/question.md).

# Question

Modify and employ the Splunk search provided at the end of this section on all ingested data (All time) to find all process names that made LDAP queries where the filter includes the string \*(samAccountType=805306368)\*. Enter the missing process name from the following list as your answer. N/A, Rubeus, SharpHound, mmc, powershell, \_

* Use this simplified search only to list all ProcessNames involved in queries with *(samAccountType=805306368)*:

```
index=main source="WinEventLog:SilkService-Log"
| spath input=Message
| rename XmlEventData.* as *
| search SearchFilter="*(samAccountType=805306368)*"
| stats count by ProcessName
| sort - count
```
