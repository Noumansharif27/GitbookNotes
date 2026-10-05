> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/intermediate-network-traffic-analysis/arp-scanning-and-denial-of-service/question.md).

# Question

Inspect the ARP\_Poison.pcapng file, part of this module's resources, and submit the first MAC address that was linked with the IP 192.168.10.1 as your answer.

* Open `ARP_Poison.pcapng` in Wireshark.
* Use this display filter to show ARP packets where `192.168.10.1` appears as the protocol (IP) address in the ARP packet:

```
arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1
```
