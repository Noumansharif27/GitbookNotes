> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/skills-assessment.md).

# Skills Assessment

#### Question 1

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the process that created remote threads in rundll32.exe. Answer format: \_.exe

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=8 TargetImage=*rundll32.exe
| stats count by SourceImage, TargetImage
```

***

#### Question 2&#x20;

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through SPL searches against all data the process that started the infection. Answer format: \_.exe

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FfDT5o0wz3wEEg1tm0Umm%2Fimage.png?alt=media&amp;token=d7897a15-bada-4746-86f3-7f7bd0ed885b" alt=""><figcaption></figcaption></figure>
