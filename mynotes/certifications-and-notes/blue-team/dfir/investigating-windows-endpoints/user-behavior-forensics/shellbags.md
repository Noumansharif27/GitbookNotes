> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/user-behavior-forensics/shellbags.md).

# Shellbags

#### **1. Introduction**

Shellbags are Windows artifacts used to store the folder view settings accessed by the user in Windows Explorer.\
They can reveal:

* Which folders were accessed (including deleted folders)
* When the access occurred (timestamps)
* Folder view properties (size, position, view mode, icons, etc.)

They are useful for forensic analysis because they help reconstruct **user behavior**, such as:

* Navigation through sensitive directories
* Access to removable media
* Access to hidden folders
* Access to network paths

#### **Shellbags Locations**

```
NTUSER.DAT\Software\Microsoft\Windows\Shell
NTUSER.DAT\Software\Microsoft\Windows\ShellNoRoam
UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\Bags
UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
```

***

#### **2. Tools**

**ShellBags Explorer**

* GUI tool to analyze Shellbags
* Allows viewing timestamps, deleted folder markers, and folder hierarchy
* Supports exporting to CSV/JSON

**Shellbags analysis using RegRipper**

* Command-line tool using the *shellbags* plugin
* Extracts Shellbag data from NTUSER.DAT and UsrClass.dat

**Export Shellbags Data using SBECmd**

* CLI tool by Eric Zimmerman
* Performs complete Shellbag parsing
* Can export in CSV, JSON, TSV
* Good for automated or batch investigations

***

#### **3. Practical Example of Analyzing the Artifacts**

**Locate and extract the registry hives**

```
C:\Users\<user>\NTUSER.DAT
C:\Users\<user>\AppData\Local\Microsoft\Windows\UsrClass.dat
```

***

#### **Method 1: ShellBags Explorer**

1. Open the tool
2. Go to: **File > Load offline hive**
3. Load NTUSER.DAT and UsrClass.dat
4. The tool will show:
   * Folder paths accessed
   * Timestamps
   * Deleted folders
5. Export results using: **File → Export → CSV**

**Practical interpretation:**

* A path such as:\
  `D:\Data\Secret\Finance\`\
  indicates navigation in the D: volume
* A folder marked **Deleted = True** means it was deleted after being accessed

***

#### **Method 2: RegRipper**

**Command:**

```
rip.exe -r NTUSER.DAT -p shellbags
```

**Output includes:**

* Reconstructed folder paths
* Timestamps
* Bag and BagMRU entries

**Practical use:**

* USB navigation:\
  `\\?\Volume{GUID}\...`
* Hidden folders:\
  `C:\Users\user\AppData\Roaming\...`

***

#### **Method 3: SBECmd**

**Basic command:**

```
SBECmd.exe -d "C:\Evidence\User\" --csv "C:\Output\Shellbags"
```

**Processing specific files:**

```
SBECmd.exe -f "C:\Evidence\NTUSER.DAT" -f "C:\Evidence\UsrClass.dat" --csv "C:\Output"
```

**Output includes:**

* Full folder path
* Timestamps
* Deleted flag
* Volume GUIDs (USB devices)
* Network paths
