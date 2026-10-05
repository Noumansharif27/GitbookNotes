> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/intermediate-network-traffic-analysis/arp-spoofing-and-abnormality-detection/question.md).

# Question

Inspect the ARP\_Poison.pcapng file, part of this module's resources, and submit the total count of ARP requests (opcode 1) that originated from the address 08:00:27:53:0c:ba as your answer.

* To filter ARP packets (packet type) and then specify ARP requests (opcode 1) originating from the address 08:00:27:53:0c:ba, you can use the following filter:

```
arp.opcode == 1 && arp.src.hw_mac == 08:00:27:53:0c:ba
```

* `arp.opcode == 1`: This filters only ARP packets that are requests (opcode 1).
* `arp.src.hw_mac == 08:00:27:53:0c:ba`: This filters only ARP packets whose origin (MAC address) is `08:00:27:53:0c:ba`.
