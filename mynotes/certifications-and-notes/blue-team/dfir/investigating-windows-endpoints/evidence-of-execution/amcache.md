> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/amcache.md).

# AmCache

#### 1. What Is AmCache

* **Definition**: The AmCache (Application Compatibility Cache) is a registry-based artifact that tracks metadata about application execution and installation on Windows systems.&#x20;
* It is stored in a registry hive file named `Amcache.hve` (in **REGF** format).&#x20;
* Supported on Windows 7 / Server 2008 R2 and later.&#x20;
* Use case in forensics: helps reconstruct application usage history, even for executables that were later deleted.

***

#### 2. Location & Structure

* **Default location**:\
  `C:\Windows\AppCompat\Programs\Amcache.hve`&#x20;
* **Registry Keys / Main Format Variants**:
  * **Old format** (e.g., Windows 7 / 8): uses keys like `Root\File` and `Root\Programs`.&#x20;
  * **New format** (Windows 10+): has keys under `Root\InventoryApplication`, `Root\InventoryApplicationFile`, `Root\InventoryDriverBinary`, etc.&#x20;
* **Update Mechanisms**:
  * AmCache is updated on PE execution (when a program is run).&#x20;
  * Also updated via scheduled tasks (e.g., “ProgramDataUpdater” / “Microsoft Compatibility Appraiser”).&#x20;

***

#### 3. What Data AmCache Contains

Some of the key fields and metadata you can find in AmCache entries:

| Field / Metadata                | What It Represents                                                                                 |
| ------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Executable / File Path**      | Full path to the exe or driver.                                                                    |
| **SHA-1 Hash**                  | The SHA-1 of the binary. Important: for large files, only the *first 31,457,280 bytes* are hashed. |
| **File Size**                   | Size of the file (in bytes), when recorded.                                                        |
| **Last Modified Timestamp**     | Last-modified time of the file on disk, as recorded in AmCache.                                    |
| **Program Version / Publisher** | For installed applications, you may see version info or publisher/company.                         |
| **Binary Type**                 | Whether it's an application, driver, or system binary.                                             |

***

#### 4. Forensic Value & Use Cases

* **Execution Evidence**: Even if a binary has been deleted, AmCache can prove its *past existence*.&#x20;
* **Timeline Reconstruction**: The “last modified” timestamp plus installation tracking gives analysts insight when a program was added or used.&#x20;
* **Malware / Threat Hunting**: Using the SHA-1 hash, you can check known malicious files, even if the file is gone.&#x20;
* **Detecting Portable or Removable Execution**: AmCache may show paths pointing to USB drives or other non-standard locations.&#x20;
* **Correlation with Other Artifacts**: AmCache complements Prefetch, Shimcache, and event logs to build a more complete picture.&#x20;

***

#### 5. Challenges, Caveats & Interpretation

* **SHA-1 Partial Hashing**: As noted, AmCache does not always hash the full binary — only the first \~31.4 MB, which can lead to mismatches.&#x20;
* **Format Differences**: Interpretation of entries depends heavily on the Windows version / AmCache “format” (old vs new).&#x20;
* **Timed Updates**: Since some updates happen via scheduled tasks, the “last modified” in AmCache may reflect the schedule run, not directly a user execution.
* **Overwriting / Roll-off**: Older entries may be lost over time if the AmCache structure rolls over.&#x20;
* **Not All Executions are Captured**: Some executables may never get recorded, depending on how they run or whether they go through compatibility mechanisms.&#x20;

***

#### 6. Tools & Techniques for Analysis

* **AmCacheParser** (Eric Zimmerman) — A common tool to parse `Amcache.hve` and extract structured data.&#x20;
* **RegRipper** — Use the `amcache_tln` plugin to extract relevant AmCache data during mass triage.&#x20;
* **Velociraptor** — Has a client artifact `Windows.Detection.Amcache` to collect SHA1, path, and other fields.&#x20;
* **Registry Viewers** — Tools like Registry Explorer can open the offline `Amcache.hve` hive so you can manually inspect the keys.

***

#### 7. Investigation Workflow (Forensics / IR)

1. **Collection**
   * Acquire the `Amcache.hve` file from a forensic image or live system.
   * Also gather associated transaction logs if available.
2. **Parsing**
   * Use AmCacheParser or RegRipper to dump the contents into a structured format (CSV, JSON).
   * Determine which AmCache format version you're dealing with (old vs new).
3. **Analysis**
   * Filter entries by SHA-1, paths, executables of interest.
   * Correlate “last modified” with known events (e.g., malware detection, incident timeline).
   * Identify anomalous entries: large executables no longer present, execution from non-standard paths, etc.
4. **Correlate with Other Artifacts**
   * Match AmCache entries with Prefetch files, Shimcache, Event Logs, etc.
   * Use the hash from AmCache to cross-reference with threat intelligence databases.
5. **Reporting**
   * Document each relevant entry: path, SHA-1, timestamp, size, likely execution.
   * Build a narrative: when a suspicious binary first appeared, whether it was executed, whether it persisted.
   * Provide recommended next steps: e.g., attempt to recover deleted binary, check file system, analyze related logs.
