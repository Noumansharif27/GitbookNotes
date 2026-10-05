> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/file-deletion-and-recovery/recycle-bin.md).

# Recycle Bin

#### 1. Why the Recycle Bin Matters in Forensics

* When a user deletes a file via **Windows Explorer**, the file often goes to the **Recycle Bin** instead of being immediately wiped.&#x20;
* This makes the Recycle Bin a “goldmine” for forensic investigators because it can provide:
  * The **original path** of the deleted file&#x20;
  * The **deletion timestamp**&#x20;
  * The **file size** and **original filename**&#x20;
* Even after a user empties the Recycle Bin, deleted content may still be recoverable via unallocated space or parsing Recycle metadata files.&#x20;

***

#### 2. How the Recycle Bin Works (Technical Details)

* **Location**: Each volume typically has a hidden system folder named `\$Recycle.Bin` at its root.&#x20;
* **Per-User Subfolder**: Inside `\$Recycle.Bin`, there are subfolders named after user SIDs.&#x20;
* **File Pairing**: For each deleted file, Windows creates two files:
  * **`$R...`**: Contains the actual data / content of the deleted file.&#x20;
  * **`$I...`**: Contains metadata about the deleted file (original path, size, deletion time).&#x20;
* **Older Versions (pre-Vista)**:
  * Windows XP and earlier used a single `INFO2` file to index deleted items, rather than separate `$I` files.&#x20;
  * The `INFO2` structure stores original path, size, deletion timestamp, and a unique ID.&#x20;

***

#### 3. Forensic Value of `$I` and `$R` Files

* **$I File**: Very important, because it includes:
  * Original full path (where the file was before deletion)&#x20;
  * File size&#x20;
  * Deletion timestamp (in Windows time format)&#x20;
* **$R File**: Contains the actual content. If intact, you can recover the file for further inspection.&#x20;
* **Recycle Bin Folder Timestamps**:
  * The modification time of the `\$Recycle.Bin` folder (or the user’s SID subfolder) can help indicate the last time an item was deleted or restored.&#x20;
* **Restore / Undo Behavior**:
  * According to forensic testing, when a file is restored via “Restore” or undo operations, sometimes only the `$R` file moves back, leaving the `$I` file behind.&#x20;
  * This can lead to “orphaned” `$I` files, which may be valuable evidence.&#x20;

***

#### 4. Tools & Techniques for Analysis

* **Rifiuti2**: A popular forensic tool to parse `$I` and `$R` files. It supports modern Windows recycle bin formats (Vista and later) and older INFO2 formats.&#x20;
  * Usage example: `rifiuti-vista.exe -x -z -o result.xml <SID-folder-path>`&#x20;
* **Recbin**: Another utility to parse Recycle Bin metadata, particularly `$I` files.&#x20;
* **Manual Analysis**:
  * Use a hex editor on `$I` files to extract deletion timestamp, original path, and file size.&#x20;
  * Investigate the `desktop.ini` file inside Recycle Bin directories — it may give hints about folder configuration.
* **File Carving**: If the Recycle Bin has been emptied, you can try carving the unallocated space to recover file content.

***

#### 5. Forensic Investigation Workflow

Here’s a step-by-step process you might follow when analyzing the Recycle Bin in a forensic investigation:

1. **Acquisition**
   * Acquire a forensic image of the disk / volume.
   * Ensure `\$Recycle.Bin` directory is included.
2. **Identification**
   * Navigate to `\$Recycle.Bin` → identify subfolders for each user (by SID).&#x20;
   * Collect all `$I*` and `$R*` files.
3. **Metadata Parsing**
   * Use **Rifiuti2** (or similar) to parse `$I` files. Extract original path, deletion times, file size.&#x20;
   * Export results to CSV / XML for timeline analysis.
4. **Content Recovery**
   * Copy `$R` files for examination. These are the actual deleted files.
   * If `$R` file is intact, try opening / inspecting the content (e.g., with appropriate app / hex editor).
5. **Timeline Construction**
   * Use deletion timestamps from `$I` files + Recycle Bin folder timestamp to reconstruct when deletions happened.&#x20;
   * Correlate with other artifacts: event logs, USN Journal, MFT entries, etc.
6. **Restoration / Undo Detection**
   * Identify orphaned `$I` files without matching `$R` files — these may reflect restored files or “undo” operations.&#x20;
   * Investigate whether deletion / restore pattern is suspicious (e.g., repeated delete + restore).
7. **Reporting**
   * Document each deleted file: original path, size, deletion time, whether the content could be recovered.
   * Highlight any anomalies: orphan `$I` files, repeated delete/restore, unusual file types.

***

#### 6. Challenges & Limitations

* **Shift+Delete Bypass**: If the user pressed `Shift + Delete`, the file bypasses the Recycle Bin entirely, so no `$I` / `$R` pair is created.&#x20;
* **Emptied Bin**: When the Recycle Bin is emptied, both `$I` and `$R` files are deleted; recovery depends on leftover data in unallocated space.&#x20;
* **SSD / TRIM Effects**: On SSDs with TRIM enabled, deleted data may be erased quickly, reducing recovery chances.
* **Orphaned Metadata**: `$I` files may remain without `$R` (e.g., after restore), complicating interpretation.&#x20;
* **Time Zone / Timestamp Interpretation**: The deletion timestamp in `$I` files is often in Windows FILETIME (UTC). Investigation may require converting / adjusting for local time zones.&#x20;
