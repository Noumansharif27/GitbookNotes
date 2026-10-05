> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/user-behavior-forensics/userassist.md).

# UserAssist

#### **1. Introduction**

UserAssist artifacts are Windows components that record **programs and items executed by the user** in the graphical interface (GUI).\
Windows stores this information inside ROT13-encoded registry keys.

UserAssist reveals:

* Which programs were opened
* How many times they were executed (count)
* The last time the execution occurred (timestamp)
* Full paths of executables
* Items executed via Explorer, Start Menu, Control Panel, etc.

#### Storage Location

Inside *NTUSER.DAT*:

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count
```

* Each **GUID** represents a different monitoring category (varies by Windows version).
* Entry names are stored in **ROT13** and must be decoded.

***

#### **2. Tools**

#### **RegRipper (userassist plugin)**

Command-line tool that extracts:

* Program name (decoded)
* Path
* Execution count
* Last execution timestamp

Useful for fast automation and reporting.

#### **Registry Explorer (Eric Zimmerman)**

GUI tool for advanced registry analysis:

* Automatically decodes ROT13
* Displays complete metadata
* Allows navigating the NTUSER.DAT tree
* Shows timestamps with FILETIME precision

***

#### **3. Practical Example: How to Analyze UserAssist**

**Obtain the NTUSER.DAT hive**

* Extract it from the user’s profile:

```
C:\Users\<user>\NTUSER.DAT
```

***

#### **Method 1: UserAssist using RegRipper**

**Command:**

```
rip.exe -r NTUSER.DAT -p userassist_tln
```

**Typical Output:**

* Executable name (decoded)
* Full program path
* LastExecutionTime
* Execution Count

**Interpretation:**

* Persistence or malicious activity may appear as repeated execution of specific tools.
* Suspicious tools will show recent execution timestamps.

Example:

```
C:\Users\User\Downloads\mimikatz.exe
Last executed: 2025-01-14 23:41:12
Count: 3
```

***

#### **Method 2: UserAssist using Registry Explorer**

1. Open Registry Explorer
2. File > Load hive > select NTUSER.DAT
3. Navigate to:

   ```
   Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist
   ```
4. Click on the GUID, then open **Count**

**You will see:**

* A decoded list of programs, shortcuts, or items
* Fields such as:
  * **Path**
  * **Count**
  * **Last Execution Time**
  * Item type (exe, lnk, control panel item, shell item, etc.)

**Practical Example:**

If you see:

```
Item: C:\Windows\System32\cmd.exe
Count: 89
Last Execution: 2025-02-10 16:22:10
```

This indicates **heavy use of cmd.exe**, which may suggest:

* Advanced manual activity
* Repeated command execution
* Script or batch file usage
