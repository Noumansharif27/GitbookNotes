> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/prefetch.md).

# Prefetch

#### 1. What Is Windows Prefetch

* Prefetch is a Windows performance feature: when an application runs, Windows tracks which files and resources it uses (for \~10 seconds) so that future launches are faster.&#x20;
* Prefetch files are stored in **`C:\Windows\Prefetch\`** and use the `.pf` extension.&#x20;
* File-naming convention: `<EXECUTABLE_NAME>-<HASH>.pf`, where the hash is derived from the full path of the executable.&#x20;
* Different Windows versions use different Prefetch *format versions*: e.g., 17 for XP, 30 for Windows 10.&#x20;
* On newer Windows (8+), Prefetch files may contain **up to 8 embedded timestamps** (last run times).&#x20;
* There is a limit on how many Prefetch files the OS retains: for example, Windows 8+ can keep up to \~1,024.&#x20;
* **Limitation**: Prefetch only records resource usage during the first seconds of execution, so not all file activity is captured.&#x20;

***

#### 2. Forensic Value of Prefetch

* **Evidence of Execution**: A `.pf` file indicates that an executable was run.&#x20;
* **Run Count**: How many times the application has been executed.&#x20;
* **Timestamps**: Useful to build a timeline — first run, last run(s).&#x20;
* **Referenced Files and Directories**: Prefetch records which DLLs and other files the application touched during startup, which can help identify malicious behavior.&#x20;
* **Executable Path Insight**: The hash in filename gives insight into where the executable was run from — important if an attacker ran it from a non-standard location (e.g., a temp folder).&#x20;
* **Persistence of Evidence**: Even if the executable is deleted, the Prefetch file may remain, giving evidence of past execution.&#x20;

***

#### 3. Prefetch File Structure (Low-Level / Technical)

* Prefetch files are binary structured, with a header and multiple sections.&#x20;
* Timestamps inside the prefetch are stored in **FILETIME (little-endian)**.&#x20;
* Key metadata in a prefetch file includes:
  * Executable name&#x20;
  * Hash of its path (used in filename)&#x20;
  * Run count&#x20;
  * Volume (disk) information (which volume the exe was run from)&#x20;
  * List of referenced files / DLLs used on startup&#x20;

***

#### 4. Tools for Prefetch Analysis

* **PECmd (by Eric Zimmerman)**:

  * A command-line tool for parsing `.pf` files.&#x20;
  * It extracts: created time, modified time, last run times, run count, volume info, referenced files/directories.&#x20;
  * Example usage:

  ```
  PECmd.exe -d "C:\Windows\Prefetch" --csv "C:\output\prefetch.csv"
  ```

***

#### 5. Investigation Workflow (Forensics / IR)

1. **Collection**
   * Acquire the `C:\Windows\Prefetch\` directory from a disk image.
   * Compute hashes of the `.pf` files for integrity.
2. **Parsing**
   * Use PECmd (or other tool) to parse every `.pf` file.
   * Export data (e.g., CSV, JSON) for further analysis.
3. **Timeline Construction**
   * Use embedded timestamps (up to 8 for newer Windows) + file system MACE timestamps to build a run timeline.&#x20;
   * The creation timestamp of the PF file often corresponds to the first execution.&#x20;
4. **Contextual Analysis**
   * Check the path hash in the filename to understand *where* the executable ran from; if it's from a suspicious location, raise a flag.&#x20;
   * Look at the list of referenced DLLs/files to understand which modules were loaded during startup — helpful for malware.&#x20;
   * Correlate Prefetch data with other artifacts: event logs, process execution logs (ex: Event ID 4688), AmCache, Shimcache, etc.&#x20;
5. **Anomaly Detection**
   * Multiple `.pf` files with the same executable name but different hashes → the same binary ran from *different paths*.&#x20;
   * Unusual run count (very high or very low) may indicate either frequent use or possible cover-up.
   * Missing `.pf` files for known malicious executables may indicate tampering / anti-forensics.
6. **Reporting**
   * For each suspicious `.pf`: document executable name, path hash, run count, run times, referenced files.
   * Build a narrative: “This executable ran from a temp directory X times, last run was at …, and loaded these DLLs …”
   * Assess forensic significance: is it malware, legitimate software, or possibly a user tool?

***

#### 6. Limitations & Caveats

* **Prefetch is disabled on some systems** (e.g., on certain Windows Server configurations).&#x20;
* **Timestamps can be imprecise**: the Prefetch mechanism monitors only the first \~10 seconds of execution, so not all activity is captured.&#x20;
* **Filename hash ambiguity**: the hash is derived from the path, but reversing it is not trivial.&#x20;
* **Limited retention**: old `.pf` files may be purged when the system reaches its Prefetch limit.
* **Anti-forensics**: attackers can delete or tamper with Prefetch files to hide execution.&#x20;
* **Not user-specific**: Prefetch does *not* record which user ran the program.&#x20;
