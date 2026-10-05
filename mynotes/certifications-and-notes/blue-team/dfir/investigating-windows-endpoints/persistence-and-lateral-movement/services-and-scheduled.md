> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/persistence-and-lateral-movement/services-and-scheduled.md).

# Services and Scheduled

#### 1. Why Services & Scheduled Tasks Matter in Forensics

* Both are *common persistence mechanisms*. Attackers often use services or scheduled tasks to maintain access, run malware periodically, or trigger payloads.&#x20;
* Services run in the background, often under high privilege (SYSTEM), so malicious services are powerful.&#x20;
* Scheduled Tasks can be very stealthy, especially if they trigger on system boot, user logon, or other events.&#x20;
* Even if task definitions are deleted, they might still run until the next reboot (depending on how they were configured).&#x20;
* Forensic artifacts exist in multiple locations: on-disk XML (or `.job`), registry, and event logs.&#x20;

***

#### 2. Key Artifact Locations & What to Examine

**Scheduled Tasks**

* **On-disk task definitions**:
  * `C:\Windows\System32\Tasks\` — XML files (modern Task Scheduler)&#x20;
  * For older versions (Task Scheduler 1.0): `.job` files in `C:\Windows\Tasks`&#x20;
* **Registry**:
  * `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks` — metadata for each task.&#x20;
  * `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree` — hierarchies and security descriptors.&#x20;
* **Event Logs**:
  * `Microsoft-Windows-TaskScheduler/Operational` — logs for task creation (Event ID 106), execution starting/completion, etc.&#x20;
  * Security events: if auditing is enabled, you can see task creation/deletion: e.g., 4698 (task created), 4699 (task deleted), 4700/4701 (enabled/disabled), 4702 (updated)&#x20;
* **Tampering considerations**:
  * Attackers may remove registry keys (`Tasks` / `Tree`) to hide tasks; but tasks may still run until reboot.&#x20;
  * Security Descriptor (SD) in registry may be missing or manipulated to hide task.&#x20;

**Services**

* **Registry**:
  * `HKLM\SYSTEM\CurrentControlSet\Services` — contains all installed Windows services (legitimate and malicious).&#x20;

**Event Logs**:

* Service creation event: Event ID **7045** (“A service was installed”) is very relevant.&#x20;
* **Executable / Binary**:
  * For each service in the registry, check the `ImagePath` (or equivalent) to understand what executable or script the service runs.
  * Cross-reference with file system (MFT), Prefetch, AmCache, etc., to see if the binary is suspicious or has execution history.

***

#### 3. Forensic Analysis Workflow

Here’s a step-by-step methodology for investigating Services and Scheduled Tasks:

1. **Collection**
   * From a forensic disk image or live system, collect:
     * `C:\Windows\System32\Tasks\` (or `C:\Windows\Tasks` for older)
     * System registry hive (to inspect `Services`)
     * SOFTWARE registry hive (for task cache entries)
     * Event logs, especially from *TaskScheduler Operational* and *System / Security* logs
2. **Parsing & Examination**
   * Load the registry hives in a tool like Registry Explorer.
   * For **services**: enumerate all service entries under `Services`, noting `ImagePath`, `StartType`, and other relevant fields.
   * For **scheduled tasks**: parse the TaskCache registry keys (`Tasks` and `Tree`) to map GUIDs → task names → XML definitions.
   * Read the XML task definitions under `C:\Windows\System32\Tasks` to extract: triggers, actions, principal (which user), arguments, author, timestamps.&#x20;
3. **Event Correlation**
   * Look in Task Scheduler’s event log for events such as: creation (106), start / run, completion.&#x20;
   * Look in Security logs for audit events if enabled (4698, 4700, etc.).&#x20;
   * For services, correlate with Event ID 7045 to detect new service installations.&#x20;
   * Combine with other artifacts: Prefetch, AmCache, MFT, etc., to confirm whether a service / task has executed.
4. **Anomaly Identification**
   * Identify *unusual tasks*: tasks created recently, tasks that run from strange or user-writable paths, or tasks that run with high privileges.&#x20;
   * Look for *hidden tasks*: tasks whose registry definitions have been removed but may still run until reboot.&#x20;
   * For services: suspicious service names, new or unknown services, paths to executables in non-standard directories.
5. **Persistence / Lateral Movement Detection**
   * Scheduled tasks are often used for remote persistence / lateral movement.
   * If an attacker created a task remotely (via SMB, PSRemoting, etc.), investigate for associated logon events (e.g. 4624 Type 3) around the time of task creation.&#x20;
   * Services might be used for persistence: check if the service is set to auto-start, or if its binary is untrusted.
6. **Reporting**
   * Document identified suspicious tasks / services: name, path, trigger, principal, creation time, last run (if available), etc.
   * Build a timeline of persistence: creation → modifications → executions → removals.
   * Provide recommendations: remove malicious tasks / services; harden Task Scheduler / Service permissions; monitor with SIEM for new task creation (e.g., alert on event ID 4698 or 7045).
