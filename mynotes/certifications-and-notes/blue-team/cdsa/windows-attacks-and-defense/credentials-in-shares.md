> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/windows-attacks-and-defense/credentials-in-shares.md).

# Credentials in Shares

#### Attack

The first step is identifying what shares exist in a domain. There are plenty of tools available that can achieve this, such as PowerView's `Invoke-ShareFinder`. This function allows specifying that default shares should be filtered out (such as c$ and IPC$) and also check if the invoking user has access to the rest of the shares it finds. The final output contains a list of non-default shares that the current user account has at least read access to:

```
PS C:\Users\bob\Downloads> Invoke-ShareFinder -domain eagle.local -ExcludeStandard -CheckShareAccess
```

Example Output:

```
\\DC2.eagle.local\NETLOGON      - Logon server share
\\DC2.eagle.local\SYSVOL        - Logon server share
\\WS001.eagle.local\Share       -
\\WS001.eagle.local\Users       -
\\Server01.eagle.local\dev$     -
\\DC1.eagle.local\NETLOGON      - Logon server share
\\DC1.eagle.local\SYSVOL        - Logon server share
```

A few automated tools exist, such as `SauronEye`, which can parse a collection of files and pick up matching words. However, because there are few shares in the playground, we will take a more manual approach (Living Off the Land) and use the built-in command `findstr` for this attack.

**Arguments:**

* `/s` forces to search the current directory and all subdirectories
* `/i` ignores case in the search term
* `/m` shows only the filename for a file that matches the term

**Example Commands:**

```
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pass" *.bat
```

#### Prevention

The best practice to prevent these attacks is to lock down every share in the domain so there are no loose permissions. Regular scans (e.g., weekly) on AD environments to identify any new open shares or credentials exposed in older ones are necessary.

#### Detection

Understanding and analyzing users' behavior is the best detection technique for abusing discovered credentials in shares. Event IDs to monitor include:

* **4624** for successful logon
* **4768** for Kerberos TGT requests

#### Honeypot

A honeypot user in AD environments: a semi-privileged username with a wrong password. Below is a good setup for the account:

* A service account created 2+ years ago, with the last password change at least one year ago.
* The account is still active in the environment.

Because the provided password is wrong, we would primarily expect failed logon attempts.

**Example Event IDs:**

* **4625** for failed logon
* **4771** for failed Kerberos pre-authentication
* **4776** for failed NTLM authentication
