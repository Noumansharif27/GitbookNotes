> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/lnk-files-and-jump-lists/lnk-files.md).

# LNK Files

#### 1. What Are LNK Files & Why They Matter

* **Definition**: LNK (or “Shell Link”) files are Windows shortcut files that store metadata pointing to a target file, folder, or application.&#x20;
* **Forensic Relevance**:
  * They can reveal **user activity**: which files/folders a user accessed, when, and from where.&#x20;
  * Even if the real target file is deleted, an LNK may remain and give a clue about what was there.&#x20;
  * Adversaries can abuse LNK files for **obfuscation / persistence**, e.g. “LNK Icon Smuggling” to hide malicious payloads.&#x20;

***

#### 2. Key Metadata Inside LNK Files

Some of the important data you can extract from an LNK:

* **Header Information**: timestamps (creation, access, modification) of the LNK file itself.&#x20;
* **Target Details**:
  * Original (or current) path of the target file/folder.&#x20;
  * Command-line arguments passed to the target (if any).&#x20;
  * Working directory.&#x20;
* **Volume Information**:
  * Volume serial number of the disk where the target lives.&#x20;
  * Drive type (local, network, removable).&#x20;
* **Shell Item / ID List**: LNKs include a “Target ID List” block linking to shell item IDs, which may include MFT information, GUIDs, and other path components.&#x20;
* **Extra Data Blocks**: LNK files may contain **extra data** (metadata) like:
  * Console properties (window size, hotkey)&#x20;
  * Property-store data blocks (custom metadata)&#x20;
  * Distributed Link Tracker information (“droid” identifiers) which helps determine if the target was moved or copied.&#x20;

***

#### 3. Common Locations of LNK Files

LNK files appear in multiple user-relevant paths:

* `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\` — recent documents.&#x20;
* Desktop shortcuts.&#x20;
* Startup folder (for persistence).&#x20;
* Jump lists (which are related but stored separately).&#x20;

***

#### 4. Attack / Threat Scenarios Involving LNK

* **Execution Evidence**: Because LNKs are created when a user opens a file (or application), they can be used to prove that something was launched.&#x20;
* **Obfuscation / Defense Evasion**: Attackers may embed malicious commands or use the *IconEnvironmentDataBlock* inside LNKs to hide links to malware or downloads. This technique is tracked in MITRE ATT\&CK as *Icon Smuggling*.&#x20;
* **Persistence**: A malicious LNK can be placed in a startup folder so that it executes every boot.&#x20;

***

#### 5. Tools & Techniques for Parsing LNK

* **LECmd (Eric Zimmerman)**
  * Powerful command-line tool that extracts *all* metadata from LNK files.&#x20;
  * Can output in CSV, JSON, or other formats for analysis.&#x20;
* **LnkParse3** (Python)
  * Minimalistic but very capable parser.&#x20;
  * Good at extracting even malformed LNKs; outputs JSON.&#x20;
* **TZWorks LNK Parser (lp)**
  * Command-line parser for `.lnk` and Jump List files.&#x20;
* **Velociraptor**
  * Has an artifact `Windows.Forensics.Lnk` to automatically find and parse LNKs during endpoint collection.&#x20;
* **Belkasoft X / Evidence Center**
  * GUI for analyzing LNK metadata: path, volume serial, timestamps, “droid” identifiers.&#x20;

***

#### 6. Forensic Investigation Workflow

Here’s a step-by-step guide to investigating LNK files:

1. **Collection**
   * Extract LNK files from common user paths (`Recent`, Desktop, Startup, etc.).
   * Use forensic image tools (FTK, EnCase, etc.) to list all `.lnk` files on the volume.
2. **Parsing**
   * Run **LECmd** or **LnkParse3** on the LNK files to extract metadata.
   * Use Velociraptor if you want to do this at scale on endpoints.
3. **Analysis / Interpretation**
   * Review target paths: see where each LNK points — local file, network share, USB, etc.
   * Compare LNK file timestamps (creation, access, modification) with target file timestamps to infer usage.&#x20;
   * Examine “extra data” blocks: check distributed tracker info (if present) to see if the target was moved.&#x20;
   * Look for suspicious or malicious LNKs: long or obfuscated `IconEnvironmentDataBlock`, unusual command line arguments.&#x20;
4. **Correlation**
   * Link LNK metadata with other artifacts: Prefetch, Jump Lists, Registry (MRU), event logs.
   * Build a timeline: when shortcuts were created vs when the target was last modified.
5. **Reporting**
   * Document each LNK: location, metadata, target, suspicious fields.
   * Highlight any LNK used for persistence, obfuscation, or pivoting.

***

#### 7. Challenges & Caveats

* Not all LNK files are evidence of malicious activity — many are simply user-created shortcuts.
* Some LNKs may point to paths that no longer exist (deleted target).
* LNK metadata can be manipulated, though not all fields are trivial to tamper with (e.g., distributed-tracker blocks).
* Parsing malformed or corrupted LNK files may require robust tools (like LnkParse3).
