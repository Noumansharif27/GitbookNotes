> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/detecting-windows-attacks-with-splunk/detecting-kerberoasting-as-reproasting/question.md).

# Question

Modify and employ the Splunk search provided at the "Detecting Kerberoasting - SPN Querying" part of this section on all ingested data (All time). Enter the name of the user who initiated the process that executed an LDAP query containing the "\*(&(samAccountType=805306368)(servicePrincipalName=\*)\*" string at 2023-07-26 16:42:44 as your answer. Answer format: CORP\\\_

```
index=main EventCode=4648 OR (EventCode=4769 AND service_name=iis_svc)
| dedup RecordNumber
| rex field=user "(?<username>[^@]+)"
| search username!=*$ 
| transaction username keepevicted=true maxspan=5s endswith=(EventCode=4648) startswith=(EventCode=4769) 
| where closed_txn=0 AND EventCode = 4769
| table _time, EventCode, service_name, username
```
