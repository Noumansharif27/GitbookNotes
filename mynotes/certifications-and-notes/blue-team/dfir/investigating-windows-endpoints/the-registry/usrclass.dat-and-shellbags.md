> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/the-registry/usrclass.dat-and-shellbags.md).

# UsrClass.dat & ShellBags

#### 1. Understanding What You're Looking At

1. **What is `UsrClass.dat`?**
   * It’s a registry hive file under the user’s profile that holds, among other things, *ShellBags* data.&#x20;
   * On modern Windows (Vista, 7, 10, 11), ShellBags are stored in both `NTUSER.dat` and `UsrClass.dat`.&#x20;
   * The relevant registry paths inside `UsrClass.dat` are:
     * `Local Settings\Software\Microsoft\Windows\Shell\BagMRU`&#x20;
     * `Local Settings\Software\Microsoft\Windows\Shell\Bags`&#x20;
2. **What are ShellBags?**
   * They are registry entries that record how a user has viewed folders (in Windows Explorer or Open/Save dialogs): window size, view mode, column sorting, etc.&#x20;
   * More importantly for forensics: ShellBags can show *which folders* were accessed, even if those folders have been deleted or were on external/removable storage.&#x20;
   * The two main subkeys are:
     * `BagMRU`: stores the folder hierarchy and MRU (most recently used) ordering.&#x20;
     * `Bags`: stores view settings (size, position, view mode) per folder.&#x20;
3. **Timestamps & Metadata**
   * ShellBags entries often include timestamps (first accessed, last modified) that help build a timeline.&#x20;
   * The MRUListEx value in BagMRU tells you the order in which child folders were accessed.&#x20;
   * NodeSlot values link to entries in `Bags` for viewing preferences.&#x20;

***

#### 2. Preparation / Collection

1. **Acquire the Hive Files**
   * From a forensic image, extract `UsrClass.dat` from the path `C:\Users\<User>\AppData\Local\Microsoft\Windows\UsrClass.dat` (or similar).&#x20;
   * Also consider getting associated transaction logs (`.LOG1`, `.LOG2`) — transaction logs may contain changes not yet committed to the hive (“dirty hive”).&#x20;
2. **Backup / Make Working Copies**
   * Before parsing, make a copy of the hive (and logs) to work on, so you preserve original forensic evidence.

***

#### 3. Tools for Analysis

Here are some common tools and their usage:

* **ShellBag Explorer (Eric Zimmerman)**
  * GUI tool to load `UsrClass.dat` (or `NTUSER.dat`) and parse ShellBag entries.&#x20;
  * It can parse both clean and “dirty” hives (if transaction logs are present).&#x20;
* **SBECmd.exe**

  * Command-line tool from Eric Zimmerman. For example:

  ```
  SBECmd.exe -f "path\to\UsrClass.dat" –csv output.csv  
  ```

  * Generates a CSV with BagPath, NodeSlot, MRUPosition, etc.&#x20;
* **Registry Explorers / Other Tools**
  * Use registry viewers that understand binary data in Bags / BagMRU.&#x20;
  * For timeline correlation, you might export the parsed data and then map it to other artifacts (MFT, event logs, jump lists).

***

#### 4. Step-by-Step Analysis Workflow

Here is a possible workflow for analyzing ShellBags via `UsrClass.dat`:

1. **Load the Hive**
   * Open the `UsrClass.dat` file with your tool (ShellBag Explorer / SBECmd).
   * If the hive is “dirty” (i.e., has unmerged transaction logs), make sure you load with the logs or use a tool that can replay them.&#x20;
2. **Parse BagMRU**
   * Navigate to `Local Settings\…\Shell\BagMRU` in the registry structure.
   * Read the MRUListEx value to understand the order of access.&#x20;
   * Inspect child keys — each “bag” corresponds to a folder (or virtual folder).
3. **Parse Bags Subkey**
   * For each BagID (i.e., folder), go into the `Bags` subkey.
   * Extract view-related metadata: window size, position, view mode, column layout, etc.&#x20;
   * Also check timestamps: “last write” or other time metadata to estimate interaction times.&#x20;
4. **Map BagIDs to Paths**
   * Using results from BagMRU, map BagIDs to actual folder paths. This way, you can know *which folder* each Bag refers to.&#x20;
   * Confirm full paths, even for deleted or disconnected folders. ShellBags may persist for folders that don’t exist anymore.&#x20;
5. **Correlate with Other Evidence**
   * Build a timeline: combine ShellBag events (first/last accessed) with other artifacts like MFT entries, file activity, event logs.
   * Use the view settings (from `Bags`) to infer user behavior: e.g., if someone always viewed a folder in “Details” mode sorted by “Date Modified,” it might indicate frequent review.
   * If a Bag indicates a removable or network path (e.g., USB drive), that’s strong forensics evidence: ShellBags can show paths to external devices.&#x20;
6. **Document Findings**
   * For each BagID, document: the folder path, view preferences, MRU order, timestamps, and any relevant metadata.
   * Note any anomalies: e.g., Bag for a path that no longer exists, or unusually deep MRU ordering, or folder settings that persist even after deletion.

***

#### 5. Interpretation & Forensic Value

* **Folder Existence Proof**: Even if a folder was deleted, the presence of a ShellBag entry proves that the folder once existed (or was at least “seen” by Explorer).&#x20;
* **User Intent & Behavior**: View settings provide insight into how the user interacted with a folder: e.g., did they browse deeply, or just glance at it?
* **Timeline Construction**: Timestamps help build a history of when a folder was first interacted with and last modified by the GUI.&#x20;
* **External or Deleted Media**: ShellBags can retain references to folders on USB drives or network shares, even if the device is no longer connected.&#x20;
* **Corroboration**: Use ShellBags alongside other artifacts to validate user behavior and paths (for example, by correlating with file system metadata or shadow copies).
