> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/detecting-attacker-behavior-with-analytics/question.md).

# Question

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an analytics-driven SPL search against all data the source process images that are creating an unusually high number of threads in other processes. Enter the outlier process name as your answer where the number of injected threads is greater than two standard deviations above the average. Answer format: \_.exe

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=8
| bin _time span=1h
| stats count as thread_count by _time, SourceImage
| eventstats avg(thread_count) as avg_count, stdev(thread_count) as stddev_count
| eval threshold=avg_count + (2 * stddev_count)
| where thread_count > threshold
| sort - thread_count
| table _time, SourceImage, thread_count, avg_count, stddev_count, threshold
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F4Uor8coJby8xXCCYaHPr%2Fimage.png?alt=media&amp;token=bb01a8b3-f49b-4536-85ef-d1da743f059a" alt=""><figcaption></figcaption></figure>
