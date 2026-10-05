> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/working-with-ids-ips/skills-assessment-snort.md).

# Skills Assessment - Snort

There is a file named wannamine.pcap in the /home/htb-student/pcaps directory, which contains network traffic related to the Overpass-the-hash technique which involves Kerberos encryption type downgrading. Replace XX with the appropriate value in the last content keyword of the rule with sid XXXXXXX within the local.rules file so that an alert is triggered as your answer.

* I used the ‘kerberos’ filter in Wireshark, which showed many packets including ones that mentioned PREAUTH\_FAILED. Looking closer into the PA data which shows the NTML Value that is not as complex as it should be
