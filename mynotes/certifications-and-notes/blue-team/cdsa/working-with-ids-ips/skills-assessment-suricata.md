> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/working-with-ids-ips/skills-assessment-suricata.md).

# Skills Assessment - Suricata

#### Question

There is a file named pipekatposhc2.pcap in the /home/htb-student/pcaps directory, which contains network traffic related to WMI execution. Add yet another content keyword right after the msg part of the rule with sid 2024233 within the local.rules file so that an alert is triggered and enter the specified payload as your answer. Answer format: C\_\_\_\_e

* Then after reading the pcap file within Wireshark, I also read the hyperlink that was mentioned in the Skills Assessment. The hyperlink mentioned that when detecting this attack to check for keywords such as ‘Win32\_Process’ and ‘Create’. Seeing how that the rule already mentions ‘Win32\_Process’ I decided to try ‘Create’.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FzVBF8dUNHHlbrtw3vvxw%2Fimage.png?alt=media&amp;token=a7ead1e3-b252-4a3c-a83c-00bc8a7382ab" alt=""><figcaption></figcaption></figure>

* Following that I ran the command

```
sudo suricata -r /home/htb-student/pcaps/pipekatposhc2.pcap -l . -k none
```

* Then I checked the log to see if the detected the attack successfully.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FUHL7dGn7uFNVEt3y7hfS%2Fimage.png?alt=media&amp;token=d5f501c3-6c24-446e-9ce9-be029e07a5b8" alt=""><figcaption></figcaption></figure>
