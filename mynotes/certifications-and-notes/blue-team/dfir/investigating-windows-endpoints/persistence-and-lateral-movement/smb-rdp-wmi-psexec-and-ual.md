> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/persistence-and-lateral-movement/smb-rdp-wmi-psexec-and-ual.md).

# SMB, RDP, WMI, PsExec & UAL

#### 1. Why These Matter in Forensics / Lateral Movement

* **SMB (Server Message Block)**\
  Attackers often abuse SMB admin shares (like `ADMIN$`, `C$`) to move laterally, copy tools / malware, or execute remote payloads.&#x20;
* **RDP (Remote Desktop Protocol)**\
  Provides interactive remote access. Adversaries may establish RDP sessions to pivot or maintain persistence.&#x20;
* **WMI (Windows Management Instrumentation)**\
  Used for remote code execution, process creation, or persistence. Highly attractive because it's a legitimate admin tool.&#x20;
* **PsExec**\
  PsExec (Sysinternals) is commonly misused for lateral execution: upload a payload via SMB then remotely run it via PsExec.&#x20;
* **UAL (User Access Logging)**\
  On Windows Server, UAL logs which user accounts accessed which “server roles” (e.g., File Server) + source IP. Great for forensic tracking of SMB / pipe usage.&#x20;

***

#### 2. Key Artifacts & Where to Look

| Technique        | Relevant Artifacts                                                                                                  | What to Investigate                                                                                                                                                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SMB / PsExec** | <p>- Windows Event Logs (Security)<br>- Admin Share Access Events<br>- UAL Database</p>                             | Look for Event IDs indicating share access, especially to `ADMIN$`/`IPC$` etc. Use UAL to tie SMB usage to particular users + source IP.  Also watch for evidence of PsExec service (`PSEXESVC`), or service creation events for PsExec. |
| **RDP**          | <p>- Security Event Logs (4624)<br>- Logon Type analysis</p>                                                        | Monitor for Logon Type **10** (RemoteInteractive) in event 4624 — typical for RDP. Correlate with unusual source IPs or times to spot lateral access.                                                                                    |
| **WMI**          | <p>- Process creation logs (Event IDs)<br>- WMI provider processes (e.g. <code>WmiPrvSE.exe</code>)</p>             | Look for unusual `wmiprvse.exe` process invocations, especially where it spawns “cmd.exe” or “powershell.exe” remotely. Use SIEM hunts to flag WMI-based lateral movement.                                                               |
| **UAL**          | <p>- ESE database files (<code>.mdb</code>) in <code>C:\Windows\System32\LogFiles\Sum</code><br>- SUM databases</p> | Parse UAL with **SumECmd** (Eric Zimmerman) to extract CSV of access: user, role, source IP, first/last seen, count.  Use this to map SMB or named-pipe (e.g. `\PIPE\svcctl`) usage to specific accounts.                                |

***

#### 3. Forensic / Detection Techniques

1. **PsExec / SMB Lateral Movement**
   * Monitor for creation of `PSEXESVC.exe` service.&#x20;
   * Check for service creation event ID **7045** in System logs.&#x20;
   * Monitor file share accesses (Event ID **5140**, **5145**) for `ADMIN$` or `IPC$`.&#x20;
2. **WMI-based Lateral Execution**
   * Hunt for `wmic /node:` or PowerShell WMI methods via process creation events.&#x20;
   * Look for persistent WMI event subscriptions (`__EventFilter`, `__FilterToConsumerBinding`) that may indicate persistence.&#x20;
3. **RDP Access**
   * Use logparser or SIEM to query event logs for 4624 with Logon Type = 10.&#x20;
   * Identify whether RDP was enabled/disabled (registry changes, firewall rule modifications) around the time of suspicious activity.
4. **UAL-based Lateral Movement Mapping**
   * Extract UAL data to see which account accessed which server role from what source IP.&#x20;
   * Baseline “normal” role access patterns. If a user appears in UAL for the first time or from an unusual source, it may indicate lateral movement.&#x20;
   * Combine UAL findings with other artifacts (SMB logon, process creation) to reconstruct attacker path.

***

#### 4. Investigation Workflow (Forensics / IR)

1. **Collection**
   * On relevant Windows servers, gather:
     * UAL `.mdb` files from `C:\Windows\System32\LogFiles\Sum`&#x20;
     * Security Event Logs (especially on machines used for admin tasks)
     * Sysmon (if deployed) or process logs for WMI and PsExec behavior
2. **Parsing / Extraction**
   * Use **SumECmd** to parse UAL database to CSV.&#x20;
   * Use SIEM or Log Parser to query event logs for SMB share accesses, process creations, and RDP logins.&#x20;
   * Extract WMI-related execution events (processes, subscriptions).
3. **Analysis**
   * Map UAL entries by user and source IP → identify unusual SMB usage.
   * Analyze process chains: PsExec or WMI parent → child processes, how they launched.
   * Build a timeline: when access happened, from where, by which account, what was executed.
4. **Correlation**
   * Correlate UAL data with event logs for process creation (e.g. PsExec), network share access, and RDP authentications.
   * Link lateral movement trace: e.g., user X accessed SMB via UAL → process created remotely on target → payload executed.
5. **Reporting**
   * Document suspicious or high-risk lateral movement activity: accounts used, source IPs, protocols, commands.
   * Highlight potential “beachhead” machines and compromised accounts.
   * Recommend mitigation: restrict admin shares, enforce logging, limit WMI / PsExec usage, monitor UAL.

***

#### 5. Challenges & Limitations

* **Legitimate Admin Activity**: Admins use SMB, PsExec, WMI, and RDP legitimately — differentiating malicious from normal is hard.
* **UAL Visibility**: Not all server roles may be logged, depending on configuration.&#x20;
* **Database Corruption**: UAL ESE databases may be in use, requiring repair (`esentutl.exe`) before parsing.&#x20;
* **Log Gaps**: If event logging (security, process, WMI) wasn’t enabled or retained, some activity may be missing.
* **Time Skew / Correlation**: Logs from different machines and artifacts may have different time sources, making correlation harder.
