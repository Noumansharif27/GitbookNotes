> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/lnk-files-and-jump-lists/jump-lists.md).

# Jump Lists

#### 1. What Are Jump Lists & Why They Matter

* Jump Lists are a Windows feature (since Windows 7) that shows recently or frequently used files / actions for applications in the taskbar.&#x20;
* For forensics, they provide **application-specific user activity**: which documents were opened, how often, and when.&#x20;
* They are **user-specific**: stored under the user profile, so you can attribute entries to a particular account.&#x20;
* There are two main types:
  1. **AutomaticDestinations** — Windows-generated MRU (Most Recently Used) lists.&#x20;
  2. **CustomDestinations** — where applications or users pin items (or define custom "jump" tasks) manually.&#x20;

***

#### 2. Where Jump Lists Are Stored

* **AutomaticDestinations** files:\
  `C:\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\*.automaticDestinations-ms`&#x20;
* **CustomDestinations** files:\
  `C:\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\*.customDestinations-ms`&#x20;
* Also, there is a registry value for "StartPage2 ProgramsCache" in:\
  `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\StartPage2` for certain Jump List related data.&#x20;

***

#### 3. Internal Structure & Key Metadata

**AutomaticDestinations** (`.automaticDestinations-ms`)

* These are **OLE Compound File (CFB)** (same as structured storage).&#x20;
* They contain:
  * Several streams: each is numbered in hexadecimal (e.g., “1a”) that corresponds to LNK-like entries.&#x20;
  * A special stream called **`DestList`**, which holds the MRU/MFU list of entries.&#x20;
* In the `DestList` entries, forensic-relevant fields include:
  * The application’s **AppID** (filename is based on this).
  * Last modification time of each jump entry (FILETIME).&#x20;
  * The path to the target, number of “pinned” uses, and frequency.&#x20;
  * Information about the target volume: serial number, whether the target was local or on a network.&#x20;
  * Sometimes system info: e.g., NetBIOS name of the machine, MAC address.&#x20;

**CustomDestinations** (`.customDestinations-ms`)

* These are basically a concatenation of **MS-SHLLINK** (LNK) records.&#x20;
* Metadata includes: target path, arguments, working directory, timestamps, etc.&#x20;
* Do **not** necessarily have MRU structure like AutomaticDestinations; “pinned” or manually created entries.&#x20;

***

#### 4. Forensic Value / Use Cases

* **User Activity Profiling**: Knowing which files an app accessed — more precise than general Recent / RecentDocs keys.&#x20;
* **Execution Evidence**: Even if the target file was deleted, Jump List may still contain reference.&#x20;
* **Timestamps**: The `DestList` stores metadata such as when an entry was first added, last used, and frequency.&#x20;
* **App Attribution**: Each Jump List filename is an **AppID**. Analysts can map AppID to app name to attribute entries to specific software.&#x20;
* **Host & Volume Info**: Jump Lists sometimes contain info about the volume (serial number) and host (NetBIOS), helping to determine where (which disk) and on which machine the item was accessed.&#x20;

***

#### 5. Tools & Methods for Analysis

* **JLECmd / JumpList Explorer (Eric Zimmerman)**: parses Jump List files, extracts LNKs, outputs CSV / other formats.&#x20;
* **TZWorks Jump List Parser**: a specialized parser.&#x20;
* **Forensic Suites** (Autopsy, X-Ways, etc.): many support Jump List parsing.&#x20;
* **Manual Analysis**:
  * Use a structured storage viewer / hex editor to examine the OLE compound file, inspect `DestList` stream.&#x20;
  * Extract embedded LNK streams and parse them using a LNK parser to get target metadata.&#x20;

***

#### 6. Investigation Workflow (Forensics / IR)

1. **Collection**
   * From a forensic image or live endpoint, navigate to:\
     `C:\Users\<User>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\`\
     and\
     `C:\Users\<User>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\`&#x20;
   * Collect all `.automaticDestinations-ms` and `.customDestinations-ms` files.
2. **Parsing**
   * Run **JLECmd** (or other parser) on the Jump List files to extract: `DestList` entries, LNK data, MRU counts, timestamps.&#x20;
   * For custom Jump Lists, parse embedded LNK entries.
3. **Analysis**
   * Match each LNK entry: look at the target path, arguments, volume info.
   * Use the `DestList` stream to build a timeline: when entries were added, when last used, how often.
   * Map AppIDs to known applications (use AppID lists / lookup tables).&#x20;
   * Identify anomalies: e.g., Jump List entries for remote paths (SMB), unusual file types, or high-frequency access.
4. **Correlation**
   * Correlate Jump List data with:
     * LNK files in Recent folder
     * Prefetch / AmCache / Shimcache
     * Event logs (file open, execution)
   * Use timestamps from `DestList` to link to other artifacts in timeline.
5. **Reporting**
   * For each Jump List, report: AppID, associated application, list of targets, timestamps, frequency.
   * Highlight suspicious entries: deleted target files, remote shares, high frequency.
   * Include a timeline of usage and access.

***

#### 7. Challenges & Caveats

* **AppID Identification**: Jump List filenames are AppIDs (hex), not readable app names — you need mapping to know which app it is.&#x20;
* **Corruption / Incomplete Data**: Not all applications update their Jump Lists in the same way; some entries may be missing or inconsistent.&#x20;
* **Pinned Items**: CustomDestinations might include items not recently used, but pinned — which may skew “recent file” analysis.
* **Time Interpretation**: Timestamps in the `DestList` are in FILETIME format; analysts should convert and correlate carefully.
* **Deleted LNK Targets**: Even if the original file was deleted, the LNK entry in the Jump List remains — analysts should confirm if the target still exists or was removed.
