> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/anatomy-of-ntfs/macb-timestamps.md).

# MACB Timestamps

### What Are MACB Timestamps

* **MACB** stands for **Modified**, **Accessed**, **Changed**, and **Birth** (sometimes Creation).&#x20;
  * **M (Modified):** When the file content was last modified.&#x20;
  * **A (Accessed):** When the file was last read.&#x20;
  * **C (Changed):** When the file's metadata (like permissions, name, or MFT record) was last changed. On NTFS this is often referred to as “MFT record changed.”&#x20;
  * **B (Birth):** The file creation time (when the file was first created).&#x20;
* On some file systems (especially Unix-like ones), only **MAC** (no “B”) are available.&#x20;

***

### How MACB Timestamps Are Stored in NTFS

* In NTFS, MACB times are stored in **at least two** attributes of an MFT (Master File Table) record:
  1. `$STANDARD_INFORMATION` attribute
  2. `$FILE_NAME` attribute&#x20;
* Each of these attributes has its own set of MACB timestamps.&#x20;
* Because there are multiple places that store similar timestamps, you can compare them for **timestomping detection**:
  * The `$STANDARD_INFORMATION` MACB timestamps are more easily modified by regular processes / anti-forensics tools.&#x20;
  * The `$FILE_NAME` MACB timestamps are much harder to tamper with (because they’re usually modified only by the kernel).&#x20;

***

### Behavior & Timestamp Rules (NTFS)

* Some rules govern when each of the MACB timestamps will be updated, depending on file operations: creation, copy, rename, move, access, etc.&#x20;
* Important nuance: **Access timestamp (A)** update is not always reliable or enabled. On NTFS, the registry key `NtfsDisableLastAccessUpdate` can control whether last access updates happen.&#x20;
* When you move a file on the *same NTFS partition*, the “Changed” timestamps (`C`) in both `$STANDARD_INFORMATION` and `$FILE_NAME` attributes update to reflect the move.&#x20;
* If you copy a file between NTFS volumes:
  * The new file inherits some timestamps (like “Modified” and “Changed”) from the original.&#x20;
  * The “Access” and “Birth” (creation) times may reflect the time of the copy, not the original.&#x20;

***

### Forensic Value

* **Timeline Analysis:** MACB timestamps are very valuable for building forensic timelines — you can see when a file was created, when it was read, when metadata changed, and when content was modified.&#x20;
* **Timestomping Detection:** Since `$STANDARD_INFORMATION` is easier to change than `$FILE_NAME`, comparing the two can reveal potential tampering. For example, if `$STANDARD_INFORMATION` shows older times than `$FILE_NAME`, it’s suspicious.&#x20;
* **High Precision:** On NTFS, timestamps are stored with very high precision (100-nanosecond intervals) as part of the NTFS metadata.&#x20;
* **Anti-Forensics Awareness:** Attackers may use tools like **TimeStomp** to modify MACB times. Detecting this often involves checking for inconsistencies, such as mismatched timestamps or abnormal patterns.&#x20;

***

### Challenges / Caveats

* Because there are **multiple attributes** (SI and FN), interpreting MACB timestamps requires care — you need to know which attribute a timestamp came from.&#x20;
* Not all file system operations update all timestamps in straightforward ways — some operations may only change certain MACB fields, depending on the OS and configuration.&#x20;
* The “Access” timestamp (A) may be disabled or not updated frequently, depending on system settings.&#x20;
* High-precision timestamps (nanosecond) can be manipulated more subtly, making anti-forensic timestomping harder to detect.&#x20;

***

### Tools & Techniques

* **nTimeTools**: A tool that can read (and modify) MACB timestamps with **100-nanosecond** precision.&#x20;
* **MFTECmd / Other MFT Parsers**: Use MFT parsing tools to extract MACB values from both `$STANDARD_INFORMATION` and `$FILE_NAME`.
* **Timeline Correlation**: Compare MACB times with other artifacts (e.g., USN Journal, Event Logs) to validate or question file activity / timestomping.
