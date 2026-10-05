> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/working-with-ids-ips/suricata-fundamentalsp/question.md).

# Question

#### Question 1

Filter out only HTTP events from /var/log/suricata/old\_eve.json using the the jq command-line JSON processor. Enter the flow\_id that you will come across as your answer.

```
cat /var/log/suricata/old_eve.json | jq -c 'select(.event_type == "http")' | head -1 | jq .
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCgxzUYsPktRSfmPMDE1r%2Fimage.png?alt=media&amp;token=d3def48a-db2e-48a5-8dba-a4af3458e246" alt=""><figcaption></figcaption></figure>

***

#### Question 2

Enable the http-log output in suricata.yaml and run Suricata against /home/htb-student/pcaps/suspicious.pcap. Enter the requested PHP page as your answer. Answer format: \_.php

* Enable http-log output in suricata.yaml

```
vim /etc/suricata/suricata.yaml
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FAETCj4ovXzMkCkx5Vv6l%2Fimage.png?alt=media&amp;token=48f2f3ee-2ab5-48f5-afd0-6a73928b9577" alt=""><figcaption></figcaption></figure>

* Run Suricata in /home/htb-student/pcaps/suspicious.pcap

```
suricata -r /home/htb-student/pcaps/suspicious.pcap -k none -l .
```

* `cat eve.json | jq | grep .php` - This filter shows all .php entries.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FOblzMayZJ3JJ1EJFCMOk%2Fimage.png?alt=media&amp;token=c2ce462d-7fab-486f-af3a-29b8ce6b6afb" alt=""><figcaption></figcaption></figure>
