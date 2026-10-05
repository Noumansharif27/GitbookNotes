> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/intro-to-network-traffic-analysis/packet-inception-dissecting-network-traffic-with-wireshark.md).

# Packet Inception, Dissecting Network Traffic With Wireshark

#### Lab Objectives

* Practice filtering captured network traffic to extract meaningful data.
* Identify servers answering DNS and HTTP/S requests.
* Analyze traffic patterns and connections.

| **Task**                                        | **Description**                                                                      | **Command/Details**                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Task 1: Read a Capture File Without Filters** | Begin by examining the `.pcap` file without applying any filters.                    | `tcpdump -r (file.pcap)`                                                                                                                                                                                                                                                                                                                                                                |
| **Task 2: Identify Traffic Types**              | Examine the traffic to identify protocols and ports.                                 | <p>- <strong>Common Protocols</strong>: DNS, HTTP, HTTPS<br>- <strong>Ports Utilized</strong>: 53 (DNS), 80 (HTTP), 443 (HTTPS)</p>                                                                                                                                                                                                                                                     |
| **Task 3: Identify Conversations and Patterns** | Analyze for patterns between servers and hosts.                                      | <p>- <strong>Patterns</strong>: Connections between server and host<br>- <strong>Three-Way Handshake</strong>: Note client/server ports<br>- <strong>Servers</strong>: Communicate over well-known ports<br>- <strong>Receiving Hosts</strong>: Use high random ports<br>- Command with Absolute Sequence Numbers: <code>tcpdump -S -r (file.pcap)</code></p>                           |
| **Task 4: In-Depth Capture Analysis**           | Answer questions on timestamps, DNS responses, and protocols.                        | <p>- <strong>First Conversation Timestamp</strong>: Look for first TCP handshake (SYN/SYN-ACK/ACK)<br>- <strong>DNS Server Response</strong>: IP for <code>apache.org</code><br>- <strong>Protocol</strong>: Identify via port numbers<br>- <strong>Example Commands</strong>: <code>tcpdump -r (file.pcap) -nn</code><br><code>tcpdump -r (file.pcap) src host \[host-name]</code></p> |
| **Task 5: Filter Out Non-DNS Traffic**          | Filter to isolate DNS traffic for analysis on domain names and DNS records.          | <p>- <strong>Filter for DNS Traffic</strong>: <code>sudo tcpdump -r (file.pcap) udp and port 53</code><br>- <strong>Hex and ASCII Output</strong>: <code>tcpdump -X -r (file.pcap)</code></p>                                                                                                                                                                                           |
| **Task 6: Filter for TCP (HTTP/HTTPS) Traffic** | Isolate HTTP/HTTPS traffic to identify web servers and analyze HTTP requests.        | <p>- <strong>Filter Command</strong>: <code>tcpdump -r (file.pcap) 'port 80 or port 443'</code><br>- <strong>Analyze Requests</strong>: Identify common HTTP methods (e.g., GET, POST) and response codes</p>                                                                                                                                                                           |
| **Task 7: Analyze First Conversation Server**   | Examine the server in the first conversation for application or server type details. | <p>- <strong>Command with Hex and ASCII Output</strong>: <code>tcpdump -X -r (file.pcap)</code><br>- <strong>Check Server Response</strong>: Look for clues in the HTTP response data for application/server information</p>                                                                                                                                                            |

#### Analysis Tips

Consider these questions to guide your analysis:

* What types of traffic are present (protocols, ports)?
* How many unique conversations and hosts?
* What is the timestamp of the first TCP conversation?
* How can traffic be filtered to simplify analysis?
* Which servers are responding on well-known ports?
* What types of DNS records and HTTP methods are used?
