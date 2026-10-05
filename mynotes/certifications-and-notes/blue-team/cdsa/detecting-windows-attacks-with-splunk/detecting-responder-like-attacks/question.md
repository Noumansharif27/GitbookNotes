> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/detecting-windows-attacks-with-splunk/detecting-responder-like-attacks/question.md).

# Question

Modify and employ the provided Sysmon Event 22-based Splunk search on all ingested data (All time) to identify all share names whose location was spoofed by 10.10.0.221. Enter the missing share name from the following list as your answer. myshare, myfileshar3, \_

```
index=main EventCode=22 QueryResults=*10.10.0.221*
| table _time, Computer, user, Image, QueryName, QueryResults
```
