> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/srum.md).

# SRUM

#### 1. What Is SRUM

* **Definition**: SRUM (System Resource Usage Monitor) is a Windows component that tracks system resource usage over time (apps, CPU, I/O, network, energy).&#x20;
* Introduced in Windows 8 (and present in newer Windows versions) for diagnostic and performance telemetry.&#x20;
* The data is stored in an **ESE database**: `SRUDB.dat`, typically located at `C:\Windows\System32\sru\SRUDB.dat`.&#x20;
* SRUM writes data roughly every hour (or on shutdown) — so timestamps are not super granular.&#x20;

***

#### 2. What Data SRUM Stores (Forensic-Relevant Tables)

SRUM holds various “provider” tables identified by GUIDs. Key ones:

| SRUM Table                     | GUID                                     | What It Tracks / Data Stored                                                                      |
| ------------------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **Application Resource Usage** | `{D10CA2FE-6FCF-4F6D-848E-B2E99266FA89}` | Tracks apps run by the user, CPU usage (foreground/background), I/O (read/write bytes), user SID. |
| **App Timeline Provider**      | `{5C8CF1C7-7257-4F13-B223-970EF5939312}` | Tracks execution duration, approximate “end” times, and compile timestamp, along with user SID.   |
| **Network Data Usage**         | `{973F5D5C-1D90-4944-BE8E-24B94231A174}` | Records per-application network usage: bytes sent, bytes received, network interface, timestamp.  |
| **Network Connectivity**       | `{DD6636C4-8929-4683-974E-22C046A43763}` | Logs when a given app connects to a network (interface, duration, etc.).                          |

***

#### 3. Forensic Value / Use Cases

* **Application Execution Evidence**: SRUM can show which applications ran, by which user (SID), how long, and how much CPU they consumed. Useful even if other execution artifacts (like Prefetch) are missing.&#x20;
* **Network Activity / Exfiltration**: Since SRUM records bytes sent/received per app, it can help investigators identify which process may have exfiltrated data.&#x20;
* **User Attribution**: The user SID is stored in many SRUM entries, enabling linking of activity to a specific user account.&#x20;
* **Trend Analysis**: Because SRUM keeps roughly 30–60 days of history (depending on OS), you can analyze “normal” usage patterns and detect anomalies.&#x20;
* **Energy / I/O Forensics**: SRUM can also log I/O operations and possibly energy usage, which may help in understanding resource-intensive or suspicious behavior.&#x20;

***

#### 4. Limitations & Caveats

* **Timestamp Granularity**: As entries are batched (hourly), the timestamp reflects when data was inserted, not the exact moment of each event.&#x20;
* **Partial Coverage**: Not all “execution” events are captured. SRUM is not a full process logger; it's more about resource usage.&#x20;
* **Data Retention**: SRUM data is stored only for a limited period (e.g., \~30–60 days) depending on Windows configuration.&#x20;
* **Database Corruption**: The ESE database (SRUDB.dat) may get corrupted; for forensic work, you might need to repair / recover it.&#x20;
* **Interpretation Required**: High network usage in SRUM doesn’t always mean exfiltration — it could be legitimate application traffic. Must correlate with other evidence.

***

#### 5. Tools & Techniques to Analyze SRUM

* **SRUM-DUMP** by Mark Baggett: A forensic tool to parse `SRUDB.dat` and export to Excel / CSV.&#x20;
* **Velociraptor**: Has an artifact `Windows.Forensics.SRUM` to parse and report SRUM data.&#x20;
* **libesedb / ESE tools**: Since SRUM uses ESE database, forensic analysts can use ESE parsers / libraries.&#x20;

***

#### 6. Investigation Workflow (Forensics / IR)

1. **Collection**
   * Acquire the `SRUDB.dat` file from `C:\Windows\System32\sru\SRUDB.dat` on a forensic image or live system.&#x20;
   * (Optional) Also collect the `SOFTWARE` registry hive to help map interface LUIDs or other contextual data.&#x20;
2. **Parsing / Repair**
   * If the ESE DB is in a “dirty” state, use tools / ESE utilities to repair it (e.g., `esentutl`).&#x20;
   * Run **SRUM-DUMP** or Velociraptor to extract relevant tables (app usage, network usage, connectivity, timeline).
3. **Analysis**
   * **Application Usage**: Identify which apps ran, how long, how much CPU they used (foreground / background), and which user.
   * **Network Usage**: For each process, examine the bytes sent/received, and correlate with network interfaces / SIDs.
   * **Connectivity**: Look for which apps connected to networks, whether those connections coincide with suspicious data transfer.
   * **Timeline Reconstruction**: Use insertion timestamps to build a rough timeline of resource usage.
4. **Correlation**
   * Combine SRUM data with other forensic artifacts:
     * Prefetch / Shimcache / AmCache — to triangulate execution.
     * Network logs (if available) — to confirm exfiltration or communication.
     * User behavior logs / timeline — to understand who was doing what.
5. **Reporting**
   * Document: app names / paths, user SIDs, bytes sent/received, times.
   * Highlight “red flags”: apps with unusual network usage, data transfers in suspicious hours, new / unknown processes.
   * Provide a narrative: e.g., “from 14:00 to 15:00 process X was active and sent Y bytes — consistent with possible exfiltration.”
