> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/anatomy-of-ntfs/metafiles-mft-journaling-ads.md).

# Metafiles, MFT, Journaling, ADS

#### 1. Key Concepts & Definitions

1. **Metafiles (NTFS metadata files)**
   * In NTFS, several "special" metafiles store file system metadata: e.g., `$MFT` (Master File Table), `$LogFile`, `$UsnJrnl`, `$Bitmap`, etc.
   * These metafiles are themselves stored as MFT entries and are critical for understanding the structure, history, and changes on an NTFS volume.&#x20;
2. **MFT (Master File Table)**
   * The **MFT** is the core metadata structure in NTFS: each file or directory corresponds to an MFT record.
   * Each MFT record contains a set of *attributes*, such as: `$STANDARD_INFORMATION` (timestamps), `$FILE_NAME`, `$DATA` (the file’s content), and possibly others.&#x20;
   * Attributes can be *resident* (stored inside the MFT record) or *non-resident* (stored elsewhere on disk, referenced by data runs).&#x20;
3. **NTFS Journaling**
   * **$LogFile**: NTFS maintains a journal for metadata changes. Before critical metadata changes are committed, they’re first written to `$LogFile`. This helps preserve consistency in case of crashes.&#x20;
   * **USN Journal (`$UsnJrnl`)**: Tracks changes to file system objects (create, delete, modify). It is often used in forensic analysis to reconstruct file activity.&#x20;
4. **Alternate Data Streams (ADS)**
   * ADS is an NTFS feature that allows a file to have multiple data streams. The default (unnamed) stream is usually `:$DATA`, but additional named streams can exist.&#x20;
   * These streams are not visible in regular Explorer listings, and their size is not reflected in the standard file size shown to the user.&#x20;
   * ADS can be used for legitimate metadata storage, but malicious actors may hide data (scripts, malware) in ADS.&#x20;

***

#### 2. Forensic Value & Use Cases

* **Hidden Data & Steganography**: ADS provides a covert storage channel. Investigators must check for ADS to uncover hidden payloads or evidence.&#x20;
* **File Timeline & Recovery**:
  * The MFT provides creation, modification, and metadata timestamps, which are essential for timeline reconstruction.&#x20;
  * The USN Journal (`$UsnJrnl`) helps detect operations (file creation / deletion / modification) even if the file is deleted.&#x20;
* **Crash / Anti-Forensics Investigation**: The `$LogFile` journal can be used to detect incomplete operations or metadata rollback (e.g., due to system crash).&#x20;
* **Metadata Anomalies**: By analyzing MFT attributes, you can detect anomalies like mismatched timestamps, strange attribute usage, or suspicious attribute counts (e.g., many `$DATA` attributes in one record).

***

#### 3. Forensic Analysis Techniques & Tools

1. **MFT Analysis**
   * Use tools like **MFTECmd** (Eric Zimmerman) to parse the MFT and extract attribute-level data like `$STANDARD_INFORMATION`, `$FILE_NAME`, `$DATA`, etc.&#x20;
   * Manually review MFT records: identify resident vs non-resident attributes, timestamps, and metadata flags.
   * Look for zombie (deleted) MFT records to recover information about deleted files.
2. **Journal Parsing / Replay**
   * Parse `$LogFile` to reconstruct metadata modification transactions. Because `$LogFile` is a transactional journal, it can help you understand what metadata changes (e.g., file renames, attribute changes) happened.
   * Use `$UsnJrnl` to build a record of file system changes (create, delete, modify). This is particularly useful for timeline reconstruction during investigations.
3. **Alternate Data Streams (ADS) Analysis**
   * Enumerate ADS on files using tools: built-in Windows APIs (`FindFirstStreamW` / `FindNextStreamW`), or forensic tools like **ADS Examiner**.&#x20;
   * Read the content of named streams and analyze it: sometimes ADS contain executables, scripts, or other hidden data.
   * If a file is deleted, its ADS streams may persist in the MFT record. ADS Examiner (or similar) can help locate ADS even for deleted files.&#x20;
4. **Journaling Forensics**
   * Use ESE-based tools or specialized parsers to interpret `$UsnJrnl` entries.
   * Reconstruct a timeline of file creations, modifications, and deletions.
   * Cross-correlate journal entries with MFT and log file data to validate sequence of events and detect anomalies (e.g., metadata changes that were not “committed” cleanly).

***

#### 4. Workflow for Forensic Investigation

Here’s a suggested workflow when investigating NTFS volume with interest in metafiles, MFT, journaling, and ADS:

1. **Image Acquisition**
   * Capture a forensic image of the volume (bit-by-bit) to preserve all metadata, MFT, journal files, and ADS data.
   * Compute hashes for integrity.
2. **Initial Parsing**
   * Parse the MFT using a tool like MFTECmd to extract the list of all files, their attributes, timestamps, and any additional data streams.
   * Export output to a CSV or timeline viewer.
3. **Journal Analysis**
   * Extract and parse `$UsnJrnl` to list all file system events (create / delete / rename / modify).
   * Parse `$LogFile` (if feasible) to understand internal NTFS metadata operations (redo/undo records).
4. **ADS Enumeration & Extraction**
   * Use ADS enumeration tools (or custom scripts) to find all ADS on the volume.
   * Extract the content of each ADS; analyze it (strings, file headers, binary content) to see if it contains malicious or suspicious data.
5. **Timeline & Correlation**
   * Build a timeline combining MFT (file metadata), USN Journal (changes), and ADS activity.
   * Correlate with other artifacts: event logs, registry, prefetch, etc., to contextualize hidden data and file system activity.
6. **Anomaly Detection**
   * Look for:
     * MFT record with many `$DATA` streams.
     * ADS with executable content.
     * Journal entries (USN) showing creation or deletion of metadata without corresponding MFT record changes.
     * Metadata timestamp inconsistencies (e.g., MFT `MFT Changed` vs `Modified`).
7. **Reporting**
   * Document all found ADS, their parent files, and content.
   * Provide a timeline of metadata changes and suspicious activity.
   * Highlight any evidence of data hiding (e.g., via ADS), anti-forensic behavior (e.g., abnormal journal entries), or suspicious file system usage.
