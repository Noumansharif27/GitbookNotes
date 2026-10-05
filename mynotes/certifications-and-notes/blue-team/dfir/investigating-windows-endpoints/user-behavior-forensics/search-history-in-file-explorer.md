> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/user-behavior-forensics/search-history-in-file-explorer.md).

# Search History in File explorer

#### **1. Introduction**

WordWheelQuery is a Windows artifact that stores the user’s search history inside File Explorer.

It allows the examiner to identify:

* Search terms typed by the user
* Quantity and frequency of searches
* Indicators of user intent (e.g., *“password”, “secret”, “.zip”, “.pdf”*)
* Recent activity on the system
* Possible attempts to locate sensitive or deleted files

**Storage location:**

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

Each entry is indexed numerically:

```
0, 1, 2, 3...
```

And contains the corresponding search term.

***

#### **2. Tools**

#### **Registry Explorer (Eric Zimmerman)**

*Search History in File Explorer: WordWheelQuery key and search keywords*

Allows you to open **NTUSER.DAT** and navigate to the WordWheelQuery key.

It displays:

* Numbered list of search terms
* Chronological order (MRU)
* Associated timestamps (when available)
* Decoded and well-organized data

***

#### **3. Practical Example: Analyzing WordWheelQuery**

**Obtain the NTUSER.DAT hive**

```
C:\Users\<username>\NTUSER.DAT
```

***

#### **Method 1: Using Registry Explorer**

1. Open **Registry Explorer**
2. Go to **File → Load Hive → select NTUSER.DAT**
3. Navigate to:

```
Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

You may see something like:

**Practical Interpretation:**

* Entry **0** is the **most recent** search term.
* Searches such as `*.zip` may indicate interest in compressed files.
* Terms like *"password"*, *"secret"*, *"key"* usually indicate intentional searching.
* Sensitive search terms help identify user behavior before an incident.

***

#### **Method 2: Using RegRipper**

**Command:**

```
rip.exe -r NTUSER.DAT -p wordwheelquery
```

**Typical output:**

```
[WordWheelQuery]
0 -> "password"
1 -> "report2024"
2 -> "*.pdf"
MRUOrder: 0,1,2
```

**Interpretation:**

* Displays search terms clearly
* Shows precise execution order
* Excellent for including directly in a forensic report
