> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/working-with-ids-ips/suricata-fundamentalsp.md).

# Suricata FundamentalsP

Suricata, managed by the Open Information Security Foundation (OISF), is an open-source network security solution ideal for Network Intrusion Detection Systems (IDS), Intrusion Prevention Systems (IPS), and Network Security Monitoring (NSM). It excels in deep packet inspection and offers extensive logging, helping administrators detect and respond to suspicious activities within network traffic.

#### Suricata Operation Modes

1. **Intrusion Detection System (IDS)**: Passively monitors traffic, flags potential threats, and enhances network visibility but does not intervene.
2. **Intrusion Prevention System (IPS)**: Acts proactively by blocking suspicious traffic before it enters the network, enhancing security at the cost of added latency.
3. **Intrusion Detection Prevention System (IDPS)**: Combines IDS and IPS features, passively monitoring but also capable of sending reset packets (RST) to terminate suspicious sessions.
4. **Network Security Monitoring (NSM)**: Purely logs all network data, focusing on capturing every data transaction for forensic and retrospective analysis.

#### Suricata Inputs

* **Offline Input**: Processes stored PCAP files, suitable for retrospective analysis and rule testing.
* **Live Input**:
  * **LibPCAP**: Reads packets from network interfaces; limited in performance.
  * **NFQ**: Linux-only, inline IPS mode leveraging IPTables to pass packets to Suricata for inspection.
  * **AF\_PACKET**: Enhanced version of LibPCAP, supporting multi-threading; suitable for live analysis on compatible Linux systems.

#### Suricata Outputs

Suricata logs various outputs, including alerts, DNS requests, HTTP requests, and network flow data. Key outputs include:

* **EVE JSON**: Logs events in JSON format for compatibility with tools like Logstash, covering event types such as alerts, DNS, HTTP, and TLS.
* **Unified2**: Snort-compatible binary alert format, allowing integration with Snort tools like `u2spewfoo`.

#### Example of Viewing EVE JSON

```
less /var/log/suricata/old_eve.json
```

#### Configuring Suricata & Custom Rules

1. **Listing Rule Files**: View available rule files.

```
ls -lah /etc/suricata/rules/
```

1. **Modifying Suricata Variables**: Define `$HOME_NET` and `$EXTERNAL_NET` in `suricata.yaml` to represent trusted and untrusted network segments, respectively.
2. **Adding Custom Rules**:

   * Example rule to alert on HTTP transactions:

   ```
   alert http any any -> any any (msg:"FILE store all"; filestore; sid:2; rev:1;)
   ```

#### Hands-on with Suricata Inputs

* **Offline Analysis**:

  ```
  suricata -r /home/htb-student/pcaps/suspicious.pcap
  ```
* **Live Input using AF\_PACKET**:

  ```
  sudo suricata --af-packet=ens160
  ```
* **Using `tcpreplay` to Simulate Traffic**:

  ```
  sudo tcpreplay -i ens160 /home/htb-student/pcaps/suspicious.pcap
  ```

#### Suricata Logs

1. **EVE JSON**: A comprehensive JSON format log containing event types like alerts, HTTP, DNS, and TLS metadata.

   ```
   less /var/log/suricata/old_eve.json
   ```

   * To view only alert events:

   ```
   cat /var/log/suricata/old_eve.json | jq -c 'select(.event_type == "alert")'
   ```
2. **fast.log**: Text-based log recording alerts only, useful for quick review.

   ```
   cat /var/log/suricata/old_fast.log
   ```
3. **stats.log**: Displays statistics and resource usage, useful for performance monitoring.

   ```
   cat /var/log/suricata/old_stats.log
   ```

#### File Extraction

Suricata can extract files transferred over protocols for forensic analysis.

1. **Enabling File Extraction** in `suricata.yaml`:

   ```
   file-store:
     version: 2
     enabled: yes
     force-filestore: yes
   ```
2. **Adding a Custom Extraction Rule**:

   * Example:

   ```
   alert http any any -> any any (msg:"FILE store all"; filestore; sid:2; rev:1;)
   ```
3. **Running Suricata on a PCAP**:

   ```
   suricata -r /home/htb-student/pcaps/vm-2.pcap
   ```
4. **Inspecting Extracted Files**:

{% code lineNumbers="true" %}

```
cd filestore
find . -type f
```

{% endcode %}

#### Updating and Reloading Rules

1. **Enable Live Rule Reloading**:

   ```
   detect-engine:
     - reload: true
   ```

   * Reload rules:

   ```
   sudo kill -usr2 $(pidof suricata)
   ```
2. **Updating Rulesets** with `suricata-update`:

   ```
   sudo suricata-update
   ```
3. **Listing Available Ruleset Sources**:

   ```
   sudo suricata-update list-sources
   ```
4. **Enabling Specific Rulesets**:

   ```
   sudo suricata-update enable-source et/open
   ```

#### Validating Suricata Configuration

Validate the configuration file to ensure Suricata is correctly set up.

```
sudo suricata -T -c /etc/suricata/suricata.yaml
```

#### Key Features of Suricata

* **Deep Packet Inspection**: Full inspection of packet content and headers.
* **Protocol Detection**: Supports multiple protocols, providing comprehensive network monitoring.
* **Intrusion Detection and Prevention**: Versatile modes for both passive and active defense.
* **File Extraction**: Captures files transferred over certain protocols for forensic analysis.
* **Live Rule Reloading**: Updates rules without service interruption.
* **Extensive Logging**: JSON, fast.log, and more, for customizable insights into network traffic.

Suricata's functionality makes it an effective tool for maintaining network security through vigilant and detailed monitoring of network traffic.
