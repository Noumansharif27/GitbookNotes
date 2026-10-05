> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/working-with-ids-ips/skills-assessment-zeek.md).

# Skills Assessment - Zeek

*There is a file named neutrinogootkit.pcap in the /home/htb-student/pcaps directory, which contains network traffic related to the Neutrino exploit kit sending Gootkit malware. Enter the x509.log field name that includes the “MyCompany Ltd.” trace as your answer.*

This was a simpler question in comparison to the previous skill assessments. First I used this command to generate the pcap that can be viewed in x509.log

```
/usr/local/zeek/bin/zeek -C -r /home/htb-student/pcaps/neutrinogootkit.pcap
```

After that I zoomed out of the command line a few times as reading it in its default size can have the log look more jumbled up than intended. I was able to read that the field I was looking for was ‘certificate.subject’.

<br>
