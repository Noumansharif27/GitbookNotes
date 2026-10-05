> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/intermediate-network-traffic-analysis/fragmentation-attacks/question.md).

# Question

Inspect the nmap\_frag\_fw\_bypass.pcapng file, part of this module's resources, and enter the total count of packets that have the TCP RST flag set as your answer.

Open `nmap_frag_fw_bypass.pcapng` in Wireshark and set the display filter:

```
tcp.flags.reset == 1
```
