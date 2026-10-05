> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/intrusion-detection-with-splunk.md).

# Intrusion Detection With Splunk

#### Introduction

In this module, we expand from individual log analysis to monitoring entire networks, using Windows Event Logs across multiple machines to identify potential malicious activity. We aim to filter out false positives, crafting precise queries and alerts to proactively secure the environment.

#### Ingesting Data Sources

We need access to various data sources for effective threat hunting. Options include:

* **BOTS**: Provided by Splunk with setup instructions.
* **logs.to**: Generates dummy logs in JSON format. When using logs.to data, set Indexed Extractions to JSON for accurate parsing.

**Query Example to Retrieve All Events:**

```
index="main" earliest=0
```

This dataset will contain over 500,000 events across various sourcetypes, representing multiple infections and types of attacks.

#### Effective Searching Techniques

Efficient querying is crucial for threat hunting. As SIEM data grows, so does processing time. Targeted searches help minimize resource usage and reduce irrelevant data. For instance:

* We'll start by listing all our sourcetypes to approach this as an unknown environment from scratch

```
index="main" | stats count by sourcetype
```

#### Generalized vs. Targeted Queries

1. **General Search (String Anywhere):**

   ```
   index="main" uniwaldo.local
   ```

   This search will retrieve all occurrences of the string "uniwaldo.local" across sourcetypes.
2. **Wildcard Search (Anywhere in String):**

   ```
   index="main" *uniwaldo.local*
   ```

   Slower performance due to broad search scope.
3. **Targeted Field Search:**

   ```
   index="main" ComputerName="*uniwaldo.local"
   ```

   Faster due to specific targeting, reducing resource load.

#### Identifying Sysmon Events by EventCode

Using Sysmon data, we can break down activity by EventCode, helping identify patterns indicative of attacks.

**Event Codes for Threat Detection**:

* **Event ID 1** - Process Creation (e.g., abnormal parent-child process hierarchies)
* **Event ID 3** - Network Connections (noise-heavy but useful for spotting anomalies)
* **Event ID 5** - Process Termination (helps detect suspicious process kills)
* **Event ID 6** - Driver Loaded (useful for identifying BYOD attacks)
* **Event ID 10** - Process Access (useful for memory dumps and injection detection)
* **Event ID 25** - Process Tampering (e.g., process herpadering, mini AV alert filter)

**Query Example - Identifying Suspicious Parent-Child Processes**:

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 | stats count by ParentImage, Image
```

This query reveals process chains, aiding in detecting unusual executions (e.g., `notepad.exe` launching `powershell.exe`).

#### Advanced Threat Detection and IP Investigation

To identify connections to suspicious IP addresses, we can query IP-related events:

**Query Example**:

```
index="main" 10.0.0.229 | stats count by sourcetype
```

Examining specific sources, such as Sysmon and Linux syslog, helps confirm machine interactions with external IPs, potentially signaling compromise.

#### Targeting Credential Dumping - Sysmon Event Code 10

**Query Example - Detecting Access to `lsass` Process**:

```
index="main" EventCode=10 lsass | stats count by SourceImage
```

This query helps identify unusual processes accessing `lsass.exe`, a common target for credential dumping.

#### Creating Effective Alerts

To develop reliable alerts, we focus on filtering noise and targeting high-fidelity indicators. For instance, by targeting API calls from `UNKNOWN` memory regions, we filter out common false positives.

#### Step-by-Step Alert Query

1. **Identify All `UNKNOWN` Call Stacks**:

   ```
   index="main" CallTrace="*UNKNOWN*" | stats count by EventCode
   ```
2. **Filter Known JITs, Microsoft.Net, and WOW64 Processes**:

   ```
   index="main" CallTrace="*UNKNOWN*" SourceImage!="*Microsoft.NET*" CallTrace!=*ni.dll* CallTrace!=*clr.dll* CallTrace!=*wow64* | where SourceImage!=TargetImage | stats count by SourceImage
   ```
3. **Exclude `Explorer.exe` and Group Results by Call Trace**:

   ```
   index="main" CallTrace="*UNKNOWN*" SourceImage!="*Microsoft.NET*" CallTrace!=*ni.dll* CallTrace!=*clr.dll* CallTrace!=*wow64* SourceImage!="C:\Windows\Explorer.EXE" | where SourceImage!=TargetImage | stats count by SourceImage, TargetImage, CallTrace
   ```

This method produces a robust alert system that distinguishes between legitimate JIT processes and potential threats.
