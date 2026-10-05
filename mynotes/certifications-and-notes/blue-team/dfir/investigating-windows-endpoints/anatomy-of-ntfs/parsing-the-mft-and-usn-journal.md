> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/anatomy-of-ntfs/parsing-the-mft-and-usn-journal.md).

# Parsing the MFT and USN Journal

#### What Are They & Why They Matter

1. **MFT (Master File Table)**
   * It’s the core metadata table on an NTFS volume — each file or directory gets a record.&#x20;
   * MFT records include attributes like `$STANDARD_INFORMATION` (timestamps), `$FILE_NAME`, and `$DATA`.&#x20;
   * Through MFT, you can recover: file metadata, deleted or unlinked records, alternate data streams, and reconstruct a file timeline.&#x20;
2. **USN Journal (`$UsnJrnl`)**
   * The Update Sequence Number (USN) Journal logs *all* file system metadata changes: creates, deletes, renames, writes, etc.&#x20;
   * Its path is typically `\$Extend\$UsnJrnl:$J` on NTFS.&#x20;
   * Forensics value: even if a file is later deleted or modified, the USN Journal can show a history of those actions.&#x20;

***

### Tools & Techniques for Parsing

* **MFTECmd (Eric Zimmerman)**

  * One of the most commonly used tools to parse MFT *and* the USN Journal.&#x20;
  * Example command to parse both:

  ```
  MFTECmd.exe -f <path to $Extend\$J> -m <path to $MFT> --csv <output-folder> --csvf usnjrnl.csv  
  ```

  * Options: dedupe, VSS support, output as JSON or CSV.&#x20;
* **PoorBillionaire USN-Journal-Parser (Python)**
  * A script that reads the USN Journal and outputs records in different formats (CSV, TLN, JSON).&#x20;
  * Parses fields like timestamp, file reference number, parent reference, reason flags (create, delete, write, rename).&#x20;
* **Velociraptor**
  * Provides an artifact `Windows.Forensics.Usn` to parse the USN Journal on endpoints.&#x20;
  * Allows filtering by filename, MFT ID, parent ID or time bounds.&#x20;
  * Also has a “carving” artifact if all you have is the raw `$J` file: `Windows.Carving.USNFiles`.&#x20;
* **NTFSInfo (DFIR-ORC)**
  * Tool that can walk the file system by parsing both MFT and USN.&#x20;
  * Outputs CSV, includes FRN (File Reference Number) to correlate records between MFT and USN.&#x20;
* **JP (TZWorks)**
  * Parser for the USN Journal, including carved or partially corrupted `$J`.&#x20;

***

### Forensic Investigation Workflow

1. **Collection**
   * Acquire a forensic image of the volume (or VSS snapshot).
   * Extract `$MFT` and the USN Journal (`$Extend\$UsnJrnl:$J`) from the image. Tools like FTK Imager or raw copy can help.&#x20;
   * Compute hashes for integrity.
2. **Parsing**
   * Run **MFTECmd** on the MFT to dump metadata (file names, timestamps, ADS, etc.).
   * Run **MFTECmd** (or Velociraptor / other parsers) on `$J` (USN journal) to extract change records.
3. **Analysis**
   * Correlate MFT records with USN entries using the MFT record number / file reference number.&#x20;
   * Look at **reason flags** in USN records (create, delete, rename, data overwrite, etc.) to understand filesystem operations.&#x20;
   * Build a **timeline**: when files were created, modified, renamed, or deleted.
4. **Anomaly Detection**
   * Identify ghost or zombie entries: MFT records that were deleted but still show up in the USN journal.&#x20;
   * Detect suspicious behavior: bulk file creation or deletion, renames, overwrites — may indicate malware or anti-forensic activity.&#x20;
   * Use timestamp mismatches: compare USN timestamps vs MFT timestamps to spot tampering.
5. **Reporting**
   * Document key events: file reference number, filename, operation type, timestamp, parent directory.
   * Provide a timeline of suspicious file system activity.
   * Recommend follow-up steps: e.g., carve deleted files, check alternate data streams, cross-correlate with other logs (event logs, registry).
