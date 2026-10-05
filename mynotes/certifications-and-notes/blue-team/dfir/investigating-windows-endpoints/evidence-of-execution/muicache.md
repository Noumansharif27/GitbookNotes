> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/evidence-of-execution/muicache.md).

# MUICache

#### 1. What Is MUICache

* MUICache (Multilingual User Interface Cache) is a registry-based artifact in Windows used by the MUI system, which helps Windows support multiple languages without needing separate binaries. [Z](https://nicolascoolman.eu/en/muicache-registre/?utm_source=chatgpt.com)
* From a forensic perspective, MUICache records metadata about GUI-based applications that were run by the user, such as: the executable path, a friendly name (description), and company name (if available).&#x20;
* Importantly: MUICache **does not record execution timestamps** — it’s more like a “list of known GUI executables used by a user,” not a full execution log.&#x20;

***

#### 2. Location(s) in Windows Registry

The location of MUICache has changed across Windows versions and hives:

| Windows Version          | MUICache Registry Path                                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **Vista and newer**      | `HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\MuiCache` (in **UsrClass.dat**) |
| **Pre-Vista (e.g., XP)** | `HKEY_CURRENT_USER\Software\Microsoft\Windows\ShellNoRoam\MUICache` (in **NTUSER.DAT**)                             |

***

#### 3. What Kind of Data You Can Get from MUICache

* **Executable Path**: The full path of GUI executables that were run.&#x20;
* **PE File Description**: The friendly name as found in the executable’s version resource (FileDescription).&#x20;
* **PE Company Name**: If present, the company/publisher name from the binary’s version resource.&#x20;
* **User Session Context**: Because MUICache is per-user, you can link entries to specific user registry hives / SIDs.&#x20;

***

#### 4. Forensic Value & Use Cases

* **Evidence of GUI Application Execution**: MUICache can show that GUI-based applications (not just command-line) were launched by a user, even if other artifacts (like Prefetch) are missing.
* **Deleted / Removed Software**: If a program was deleted, its MUICache entry may remain, giving you a clue about what was run previously.&#x20;
* **User Attribution**: Because the entries are stored per user (in their hive), you can potentially attribute execution to a particular user account.
* **Malware Hunting**: Suspicious or uncommon executable names in MUICache might indicate malware or unwanted software, especially if other traces are gone.&#x20;

***

#### 5. Limitations & Caveats

* **No Execution Time**: MUICache does not store when the application was executed — only that it *was* run (or at least launched) at some point.&#x20;
* **Not All Executables Appear**: Only GUI-based applications (those that present a window) may be recorded. Non-GUI or background processes may not produce MUICache entries.&#x20;
* **Can Be Cleared or Altered**: Because it’s just registry data, an attacker (or a user) could delete entries or change permissions to prevent updates.&#x20;
* **Orphaned Entries**: Applications that were uninstalled or moved may leave orphan MUICache entries (references to files that no longer exist).&#x20;

***

#### 6. Tools & Methods for Analysis

* **Dissect**: The `muicache` module in Dissect (forensic tool) can parse MUICache from registry hives.&#x20;
* **RegRipper**: Use the MUICache plugin (if available) to extract entries from the user hive.&#x20;
* **Registry Explorer / Viewer**: You can load the hive (e.g., `UsrClass.dat`) and navigate to the MUICache key to manually inspect entries.
* **MUICacheView (NirSoft)**: GUI tool that shows MUICache entries and allows editing or deleting them.&#x20;

***

#### 7. Investigation Workflow (Forensics / IR)

1. **Collection**
   * Acquire the `UsrClass.dat` hive (or other relevant user hive where MUICache is stored).
   * Also grab any live registry if possible (to see current entries).
2. **Parsing / Extraction**
   * Run a registry parser (RegRipper, Dissect, etc.) to get MUICache entries.
   * Export the data into CSV/JSON so you can analyze and filter later.
3. **Analysis**
   * Review executable paths and friendly names. Look for unusual paths (e.g., temp dirs, user profile).
   * Correlate entries with other artifacts (Prefetch, Shimcache, AmCache) to build a more complete picture of what was run.
   * Note entries that reference executables no longer present — which could indicate deletion.
4. **Report**
   * For each potentially malicious or suspicious entry: document the path, the friendly name, user context.
   * Build a timeline (if possible) of application usage / presence, even though execution time isn’t recorded in MUICache.
   * Provide recommendations (e.g., recover missing binaries, check for persistence, further forensic triage).
