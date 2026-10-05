> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/persistence-and-lateral-movement/lsass-ntds.dit-and-wdigest.md).

# LSASS, NTDS.dit & WDigest

#### 1. What These Components Are

**LSASS (Local Security Authority Subsystem Service)**

* `lsass.exe` is a core Windows process that manages local security, authentication, and policy enforcement.&#x20;
* It holds sensitive credential material in memory: NTLM / LM hashes, Kerberos tickets, and (in some scenarios) clear-text passwords.&#x20;
* Because of this, it's a common target for credential dumping by attackers.&#x20;

**NTDS.dit**

* `NTDS.dit` is the Active Directory database file on a Domain Controller. It stores user and computer account data, group membership, and most critically, password hashes.&#x20;
* Its compromise gives attackers access to **all domain credentials** (e.g., NTLM, Kerberos) in one place.&#x20;
* Dumping this file is covered by MITRE ATT\&CK as *OS Credential Dumping – NTDS*.&#x20;

**WDigest**

* WDigest is a legacy authentication protocol used in older Windows versions (HTTP / SASL) via `wdigest.dll`.&#x20;
* If enabled (via registry), WDigest causes LSASS to keep *plain-text* credentials in memory.&#x20;
* Attackers often enable this (`UseLogonCredential = 1`) to dump cleartext passwords.&#x20;

***

#### 2. Forensic / Attack Scenarios

Here’s how attackers abuse these components and how you can investigate them:

| Component    | Common Attack Technique                                                                                                                                                                 | Forensic Value / What to Investigate                                                                                                                                                                                                                                                                                                                                         |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **LSASS**    | - Memory dump (e.g. using Mimikatz, ProcDump, or other tools) to extract credentials.                                                                                                   | <p>- Acquire LSASS memory (either live or via minidump) and analyze with tools like Mimikatz.<code>sekurlsa::logonpasswords</code> can show hashes, clear-text credentials, tickets.<br>- Watch for abnormal process creation or <code>lsass.exe</code> access.<br>- Use EDR / Sysmon to detect suspicious access to LSASS.</p>                                              |
| **NTDS.dit** | <p>- Dumping the AD database to extract all user credentials. <br>- Using Volume Shadow Copy or <code>ntdsutil.exe</code> to copy <code>NTDS.dit</code> even though it's “locked.” </p> | <p>- On Domain Controllers, monitor for shadow copy (VSS) creation + access to <code>NTDS.dit</code>. MITRE’s DET0586 describes detection. <br>- After obtaining a copy, extract the SYSTEM hive to decrypt hashes, parse NTDS, and inspect account data.<br>- Correlate with other AD events/logs to understand misuse.</p>                                                 |
| **WDigest**  | <p>- Enabling WDigest in registry (<code>UseLogonCredential = 1</code>) so LSASS stores plaintext credentials. <br>- Then dumping LSASS to get clear-text passwords. </p>               | <p>- Check registry key: <code>HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest\UseLogonCredential</code>. If set to <code>1</code>, that's a big red flag. <br>- Monitor for tools / processes that dump LSASS memory after such change.<br>- Consider whether <code>RunAsPPL</code> (“protected process”) is enabled for LSASS to make dumping harder. </p> |

***

#### 3. Mitigations & Hardening

* **Disable WDigest Clear-Text Credentials**\
  Set `UseLogonCredential = 0` at: `HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest`.&#x20;
* **Enable LSASS Protection (PPL)**\
  Configure LSASS to run as a “Protected Process Light” so only signed/protected processes can access it.&#x20;
* **Secure Domain Controller Backups**
  * Limit who can access VSS/shadow copies. \<br>- Monitor for shadow copy creation and suspicious file access to `NTDS.dit`.&#x20;
* **Audit & Logging**
  * Enable PowerShell and command-line auditing to catch `ntdsutil.exe`, `secretsdump.py`, or similar tools.&#x20;
  * Log LSASS access, driver loading, and memory dump attempts (if your EDR supports it).

***

#### 4. Investigation Workflow

1. **Triage and Detection**
   * Search for registry modifications to WDigest (`UseLogonCredential`).
   * Detect Volume Shadow Copy creation around times when `NTDS.dit` might have been accessed.
   * Identify suspicious process creation (e.g. `ntdsutil.exe`, mimikatz, `secretsdump.py`).
2. **Collection**
   * If possible, take a memory dump of `lsass.exe`.
   * From a Domain Controller: acquire `NTDS.dit` (via shadow copy or offline image) and the SYSTEM hive for decryption.
3. **Parsing and Analysis**
   * Use Mimikatz (or similar) to analyze LSASS memory for credentials.
   * Use tools (e.g. `ntdsutil`, AD forensic tools) to parse `NTDS.dit`, extract NTLM/Kerberos hashes, account metadata.
   * Decrypt using the boot key from the SYSTEM hive.
4. **Correlation & Reporting**
   * Correlate credential dumps with suspicious activity (lateral movement, account misuse).
   * Document findings: which accounts were compromised, what types of credentials (plaintext, hash), when the compromise likely happened.
   * Recommend mitigations: disable WDigest, enforce LSASS protection, limit DC access, improve logging.
