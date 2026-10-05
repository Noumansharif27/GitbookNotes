> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/userassist.md).

# UserAssist

#### 1. What Is UserAssist

* **Definition**: The UserAssist registry key is a Windows artifact that tracks the GUI-based applications a user has launched via Explorer, Start Menu, desktop shortcuts, “Run” dialog, etc.&#x20;
* It records per-user data, so each user’s `NTUSER.DAT` hive will have its own UserAssist entries.&#x20;
* The data stored is obfuscated: the **value names** are ROT13-encoded.&#x20;

***

#### 2. Where to Find UserAssist in the Registry

* Path:\
  `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`&#x20;
* Important GUIDs:
  * `CEBFF5CD-ACE2-4F4F-9178-9926F41749EA`: tracks **.exe** execution via GUI.&#x20;
  * `F4E57C4B-2036-45F0-A9AB-443BCFE33D9F`: tracks execution via **shortcuts (.lnk)**.&#x20;

***

#### 3. What Data UserAssist Contains (Forensic-Relevant Fields)

UserAssist entries provide several pieces of metadata:

| Data Field                     | Description                                                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| **Application (decoded name)** | The executable or shortcut name / path (decoded from ROT13)                                  |
| **Run Count**                  | How many times the application was launched via GUI                                          |
| **Last Execution Timestamp**   | Stored as a Windows `FILETIME`; indicates the last time the app was run (or at least “used”) |
| **Focus Time**                 | How long the application window was in focus (milliseconds)                                  |
| **Focus Count**                | Number of times the application became the active (foreground) window                        |

***

#### 4. Forensic Value & Use Cases

* **User Behavior & Application Usage**: Helps reconstruct which GUI applications a user frequently used, and how often.&#x20;
* **Timeline Reconstruction**: The last run timestamp plus run count can be used to build a usage timeline.&#x20;
* **Suspicious or Malicious Application Activity**: If malware or remote-access tools were launched via GUI, UserAssist may capture them.&#x20;
* **User Attribution**: Since entries are stored per user, you can attribute application usage to a specific user account.&#x20;

***

#### 5. Limitations & Caveats

* **Only Tracks GUI Executions**: Does *not* reliably track command-line applications, background services, or scheduled tasks.&#x20;
* **ROT13 Encoding**: Entry names are encoded, so you need to decode them to interpret properly.&#x20;
* **Potential False Positives**: Opening the file location of a shortcut (right click → “Open file location”) can create a UserAssist entry *without the application actually running*.&#x20;
* **Manipulation Risk**: An attacker or user might clear or tamper with these registry keys.&#x20;
* **Limited GUIDs**: Only certain GUIDs are tracked, so not all methods of launching apps will be recorded.&#x20;

***

#### 6. Tools & Methods for Analysis

* **RegRipper**: Has a `userassist` plugin to extract and decode UserAssist entries.&#x20;
* **Volatility3**: Use the `windows.registry.userassist` plugin to extract UserAssist from memory dumps.
* **Manual via Registry Explorer**: Load `NTUSER.DAT` with a registry viewer, navigate to the UserAssist key, decode names manually.

***

#### 7. Investigation Workflow (Forensics / Incident Response)

1. **Collection**
   * Extract the user’s `NTUSER.DAT` hive from disk or memory.
   * If doing memory forensics, dump memory and run Volatility’s UserAssist plugin.
2. **Parsing**
   * Run RegRipper (or another parser) to decode ROT13 and pull out run count, last run, focus data.
   * Export parsed entries to CSV / JSON for analysis.
3. **Analysis**
   * Identify the most frequently used applications via “run count.”
   * Look at `last execution` times to correlate with incident timeline.
   * Examine focus time / focus count: non-zero focus time is a good indicator the app was actually used, not just clicked.&#x20;
   * Watch for suspicious paths (e.g., apps run from user temp folders or non-standard locations).
4. **Correlation**
   * Correlate UserAssist findings with other forensic artifacts: Prefetch, AmCache, Shimcache, Event Logs.
   * Use GUID analysis: compare entries under the `.exe` GUID vs the `.lnk` GUID to understand how the user launched the app.&#x20;
5. **Reporting**
   * Document key entries: application path, run count, last run, focus time / count.
   * Build a narrative of user activity: which apps were used, when, and how often.
   * Highlight unusual or potentially malicious usage (e.g., unknown executables, high run count in short period).
