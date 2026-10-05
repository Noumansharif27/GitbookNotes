> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/the-registry/ntuser.dat.md).

# NTUSER.DAT

#### 1. Introduction to NTUSER.DAT

**What is NTUSER.DAT?**

* NTUSER.DAT is a user-specific registry hive that stores configuration info, application settings, and user behavior artifacts.&#x20;
* It acts as a snapshot of a user’s environment and activity.&#x20;

**File Location & Lifecycle:**

* Located at `C:\Users\[Username]\NTUSER.DAT` for standard user accounts.&#x20;
* For service accounts (e.g., Local Service, Network Service), at `C:\Windows\ServiceProfiles\*\NTUSER.DAT`.&#x20;
* On logon, Windows loads it into memory and maps it to `HKEY_USERS\{SID}`.&#x20;
* At logoff or shutdown, it's unloaded and written back to disk.&#x20;
* In MSIX-based applications, there can be app-specific hives: under `%localappdata%\Packages\<APPID>\SystemAppData\Helium\User.dat`.&#x20;

**Why It Matters for Forensics:**

* Reveals which programs the user ran, the frequency, and when.&#x20;
* Tracks files accessed, user interactions with GUI, recent documents, typed paths.&#x20;
* Helps identify persistence (malware) via Run / RunOnce keys.&#x20;
* Includes timestamps that can be correlated with Prefetch, event logs, and other sources to build a timeline.&#x20;
* Because it's user-specific (HKCU), it gives context about *which user* did what, distinguishing from system-wide activity.&#x20;
* Shows user intent: typed paths, open/save history, etc., which can be powerful in investigations.&#x20;

***

#### 2. Key Artifacts & Registry Locations

Here are the main NTUSER.DAT artifacts that are useful in forensic investigations:

<table><thead><tr><th width="156.8074951171875">Artifact</th><th>Registry Location</th><th width="216.37957763671875">Purpose </th><th>Important Notes</th></tr></thead><tbody><tr><td><strong>UserAssist</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist</code></td><td>Shows evidence of execution of .lnk or GUI PE files; includes run count, last run time, focus time. </td><td>On Windows 10+, there may be entries without real execution; must interpret carefully. </td></tr><tr><td><strong>RunMRU</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU</code></td><td>Records up to ~26 recent commands typed in the "Run" dialog. </td><td>Doesn’t always guarantee execution  could just be text typed. </td></tr><tr><td><strong>LastVisitedMRU</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU</code></td><td>Tracks directories opened via “Open/Save” dialogs, per application. </td><td>Only reflects access through dialog, not all file accesses. </td></tr><tr><td><strong>OpenSaveMRU</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePidlMRU</code></td><td>Records most recently accessed files per extension via Open/Save dialogs. </td><td>Important to check app-specific hives too. </td></tr><tr><td><strong>RecentDocs</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs</code></td><td>List of recently accessed documents / files. </td><td>Subkeys by extension; MRU order + last accessed time. </td></tr><tr><td><strong>OfficeMRU</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Office\…\File MRU</code> (and User MRU for Office 365)</td><td>Files recently opened in Office apps (Word, Excel, PowerPoint). </td><td>Includes paths, last opened times, and can tie to user accounts. </td></tr><tr><td><strong>ShellBags</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU</code> &#x26; <code>…\Bags</code></td><td>Shows folder structures accessed by user, including via network shares. </td><td>In NTUSER.DAT, limited mostly to network (UNC) folders. </td></tr><tr><td><strong>WordWheelQuery</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery</code></td><td>Stores search terms used in the Explorer search box by the user. </td><td>Has MRU order + timestamp of the most recent search. </td></tr><tr><td><strong>TypedPaths</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths</code></td><td>Paths typed by the user in File Explorer's address bar. </td><td>Paths are resolved (variables, shortcuts) and stored as full paths. </td></tr><tr><td><strong>Run / RunOnce Keys</strong></td><td><code>HKCU\Software\Microsoft\Windows\CurrentVersion\Run</code> and <code>…\RunOnce</code></td><td>Programs configured to run when the user logs on, often used for persistence. </td><td>Common persistence area for malware. </td></tr><tr><td><strong>MountPoints2</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2</code></td><td>Shows USB devices or network shares the user has mounted. </td><td>Contains volume GUIDs, share paths, etc. </td></tr><tr><td><strong>Terminal Server Client (RDP)</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Terminal Server Client\Servers</code></td><td>Records RDP connections made by the user (hostname, username, MRU). </td><td>Has MRU, host info, and user hints. </td></tr><tr><td><strong>Installed Apps (User-Specific)</strong></td><td><code>NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Uninstall</code></td><td>Lists applications installed only for that user (not system-wide). </td><td>Useful to see software in user context. </td></tr></tbody></table>

Other artifacts may exist depending on specific applications (e.g., RMM tools, exfil tools). \
Useful tools for registry artifact analysis: RegSeek, RegRipper4, RECmd, etc.&#x20;

***

#### 3. Methods for Parsing NTUSER.DAT

There are two main approaches to extract data from NTUSER.DAT:

**1. Via the Running System / API**

* **Advantages:** Easy to do if the system is live, no need to extract the hive manually.&#x20;
* **Disadvantages:** The system could be tampered with (e.g., API hooking by malware).
* Tools that rely on this method require a live system.

**2. Offline / External Parsing (Hive Parsing)**

* **Advantages:** More reliable, doesn’t depend on a running system; avoids API manipulation.&#x20;
* **Disadvantages:** Must deal with transaction logs to not miss uncommitted registry changes.&#x20;
* **Tools:**
  * **Regripper** - command-line plugin-based registry parser.&#x20;
  * **Registry Explorer** - GUI, with bookmarks for common artifacts.&#x20;
  * **RECmd / RLA** - can parse registry and replay transaction logs.&#x20;
  * **Autopsy** - open-source DFIR platform; has modules for registry.&#x20;

**Reference:**&#x20;

{% embed url="<https://www.cybertriage.com/blog/ntuser-dat-forensics-analysis-2025/>" %}
