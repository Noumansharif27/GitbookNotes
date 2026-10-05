> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/shimcache.md).

# Shimcache

#### 1. What Is Shimcache / AppCompatCache

* **Definition**: Shimcache (Application Compatibility Cache) is a component of Windows’ Application Compatibility infrastructure. Originally designed to help older applications run on newer Windows versions.&#x20;
* **Purpose (for forensics)**:
  * Records metadata about executable files, such as full path, last modified time, and file size.&#x20;
  * Even if binaries are deleted, Shimcache entries can remain, giving forensic investigators evidence that a file **existed** on the system.&#x20;
  * Useful to corroborate application execution when other artifacts (like Prefetch) are missing.&#x20;

***

#### 2. Where Shimcache Is Stored

* Shimcache data is located in the **SYSTEM** registry hive.&#x20;
* Typical registry paths:
  * For Windows 7 and newer:

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache  
    ```

  * For older Windows (e.g., XP):

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatibility\AppCompatCache  
    ```

***

#### 3. What Shimcache Records (Metadata)

Shimcache entries typically include:

* **Full path** to the executable.&#x20;
* **File size** (depending on OS version)&#x20;
* **Last modified timestamp** (from standard file information)&#x20;
* **Insert / execution flag**: On some Windows versions, an “Insert Flag” or other indicators suggest whether the binary was actually executed.&#x20;
* Important: *Last modified time* in Shimcache does **not always** mean execution time—it may just reflect when the file was last changed.&#x20;

***

#### 4. Behavior & Limitations

* **Write timing**: Shimcache entries are **written to the registry only when the system shuts down or reboots** (at least on Windows 7+).&#x20;
* **Rolling buffer**: Older entries are overwritten when the capacity is reached. For example:
  * Windows XP: up to \~96 entries.&#x20;
  * Newer Windows (Vista and later): up to \~1,024 entries.
* **Execution indicator**:
  * On Windows 10 / 11, newer research suggests the **last 4 bytes** of Shimcache data may indicate execution (e.g., `01 00 00 00` = executed).&#x20;
  * But this is not always reliable, and false-negatives are possible. [1](https://artefacts.help/windows_shimcache.html?utm_source=chatgpt.com)
* **Volatility**: If the system has not shut down since the last execution, new Shimcache entries may be *only in memory*.&#x20;

***

#### 5. Forensic Value / Use Cases

* **Proving presence of files**: Even after deletion of an executable, Shimcache may show its path, indicating it was once on the system.&#x20;
* **Correlating execution**: While not definitive for execution in all cases, Shimcache helps correlate with other artifacts (Amcache, Prefetch, Event Logs).&#x20;
* **Detecting anti-forensics**: Discrepancies between the “last modified” timestamp in Shimcache and the actual file timestamp may suggest tampering or “time stomping.”&#x20;
* **Server forensics**: Prefetch may be disabled on servers, but Shimcache usually exists, making it especially valuable in server investigations.&#x20;

***

#### 6. Tools & Techniques to Parse Shimcache

* **AppCompatCacheParser** (by Eric Zimmerman) — a widely used tool to extract Shimcache entries from SYSTEM hive.&#x20;
* **ShimCacheParser.py** (by Mandiant) — Python tool for parsing various Shimcache versions.&#x20;
* **Volatility (memory forensics)**:
  * `shimcache` plugin (Volatility 2) can extract in-memory Shimcache entries (i.e., entries not written to disk yet).&#x20;
* **Manual registry analysis**: Using registry viewers (e.g., Registry Explorer) to navigate to the `AppCompatCache` key and inspect entries.&#x20;

***

#### 7. Investigation Workflow (Forensics / IR)

1. **Collection**
   * Extract the `SYSTEM` hive from a forensic image.
   * If doing memory forensics, capture a memory dump.
2. **Parsing**
   * Run AppCompatCacheParser against the SYSTEM hive to extract entries.
   * Or use `ShimCacheParser.py` to output CSV / JSON for analysis.
   * If you have memory, use Volatility to pull in-memory entries too.
3. **Analysis**
   * Identify exe paths, sizes, and last-modified timestamps from Shimcache entries.
   * Look for entries referring to suspicious or unusual paths (e.g., temp folders, network share, removable media).
   * Compare “last modified” times in Shimcache to file system timestamps (MFT / MACE) to detect inconsistencies / tampering.
   * Check for multiple entries for the *same executable name* but different paths or sizes (could indicate different versions or renamed binaries).
4. **Correlation**
   * Correlate Shimcache data with other artifacts:
     * **Prefetch**: to see execution count and run times.
     * **Amcache**: for more detailed metadata (hash, first run, install data).&#x20;
     * **Event Logs** (e.g., process creation logs).
   * Use the entry insertion order (or “cache position”) when possible to infer relative order of execution.&#x20;
5. **Reporting**
   * Document each Shimcache entry: path, size, last modified, “executed” flag (if available), and any anomalies.
   * Build a timeline of suspected execution / presence.
   * Highlight suspicious behavior: files run from non-standard locations, time discrepancies, etc.
   * Provide recommendations for further investigation (e.g., check file system for missing files, shadow copies).
