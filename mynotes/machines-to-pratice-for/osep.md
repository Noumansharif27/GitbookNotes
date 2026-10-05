> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/machines-to-pratice-for/osep.md).

# OSEP

Lista de máquinas HTB (Hack the Box) para o OSEP (PEN-300)

<details>

<summary>Client Side Code Execution With Office</summary>

3.

```
<table><thead><tr><th width="198" align="center">Attack Type</th><th align="center">HTB Machine</th><th width="177" align="center">Attack Used in HTB</th><th>Link</th></tr></thead><tbody><tr><td align="center">Phishing with Microsoft Office: RTF Document</td><td align="center">REEL</td><td align="center">malicious RTF Document (CVE-2017-0199)</td><td>Walkthrough : <a href="https://youtube.com/watch?v=ob9SgtFm6_g&#x26;t=794">https://youtube.com/watch?v=ob9SgtFm6_g&#x26;t=794</a><br>Tool: <a href="https://github.com/bhdresh/CVE-2017-0199">https://github.com/bhdresh/CVE-2017-0199</a></td></tr><tr><td align="center">Phishing with Microsoft Office: LibreOffice</td><td align="center">RE</td><td align="center">LibreOffice Macro</td><td>Walkthrough : <a href="https://youtube.com/watch?v=ob9SgtFm6_g&#x26;t=794">https://youtube.com/watch?v=ob9SgtFm6_g&#x26;t=794</a><br>Tool: &#x3C;></td></tr><tr><td align="center">Phishing/Macro</td><td align="center">REEL2</td><td align="center">Grab NTLMv2 with Malicious link</td><td>Walkthrough : <a href="https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=2350">https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=2350</a><br>Tool: &#x3C;></td></tr><tr><td align="center">Phishing with Microsoft Office: LibreOffice</td><td align="center">RABBIT</td><td align="center">LibreOffice Macro</td><td>Walkthrough : <a href="https://youtube.com/watch?v=5nnJq_IWJog&#x26;t=1935">https://youtube.com/watch?v=5nnJq_IWJog&#x26;t=1935</a><br>Tool: &#x3C;></td></tr></tbody></table>
```

</details>

<details>

<summary>Introduction to Antivirus Evasion</summary>

4.

```
|    Attack Type    | HTB Machine |                                                        Link                                                       |
```

```
| :---------------: | :---------: | :---------------------------------------------------------------------------------------------------------------: |
| Obfuscating Macro |      RE     | Walkthrough : [https://youtube.com/watch?v=YXAakamjO\_I\&t=1005](https://youtube.com/watch?v=YXAakamjO_I\&t=1005) |
```

</details>

<details>

<summary>Advanced Antivirus Evasion</summary>

5. |             |             |                                                                                                                  |
   | ----------- | ----------- | ---------------------------------------------------------------------------------------------------------------- |
   | Attack Type | HTB Machine | Link                                                                                                             |
   | Amsi Bypass | APT         | Walkthrough : <https://youtube.com/watch?v=eRnqtXwCZVs&t=4650>                                                   |
   | Amsi Bypass | PivotAPI    | Walkthrough : [https://youtube.com/watch?v=eRnqtXwCZVs\&t=4650](https://youtube.com/watch?v=FbTxPz_GA4o\&t=6360) |
   | Amsi Bypass | MULTIMASTER |                                                                                                                  |

</details>

<details>

<summary>Application Whitelisting</summary>

6.

```
<table><thead><tr><th align="center">Attack Type</th><th align="center">HTB Machine</th><th width="184" align="center">Attack Used in HTB</th><th align="center">Link</th></tr></thead><tbody><tr><td align="center">Applocker Bypass</td><td align="center">REEL2</td><td align="center">Breaking out of ConstrainedLanguage Mode by creating a function</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=3600">https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=3600</a></td></tr><tr><td align="center">Applocker Bypass</td><td align="center">GIDDY</td><td align="center">Escaping powershell constrained mode with PSBypassCLM</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=J2unwbMQvUo&#x26;t=2750">https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=360</a>0</td></tr><tr><td align="center">Applocker Bypass</td><td align="center">SEKHMET</td><td align="center">intended way of bypassing applocker</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=vsgPsMZx59w&#x26;t=6300">https://youtube.com/watch?v=vsgPsMZx59w&#x26;t=6300</a></td></tr><tr><td align="center">Applocker Bypass</td><td align="center">-</td><td align="center">AppLocker Bypass COR Profiler</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=T91iXd_VPVI&#x26;t=1650">https://youtube.com/watch?v=Ro2vXt_WFDQ&#x26;t=3600</a></td></tr></tbody></table>
```

</details>

<details>

<summary>Windows Credentials</summary>

7.

```
<table><thead><tr><th width="225" align="center">Attack Type</th><th align="center">HTB Machine</th><th width="188" align="center">Attack Used in HTB</th><th align="center">Link</th></tr></thead><tbody><tr><td align="center">Local Windows Credentials: LSASS Dump</td><td align="center">ATOM</td><td align="center">Using rundll32 to create a memory dump of LSASS</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=1OC2eRVX0ic&#x26;t=1830">https://youtube.com/watch?v=1OC2eRVX0ic&#x26;t=1830</a></td></tr><tr><td align="center">Local Windows Credentials: LSASS Dump</td><td align="center">BLACKFIELD</td><td align="center">running pypykatz to extract credentials</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=IfCysW0Od8w&#x26;t=2205">https://youtube.com/watch?v=1OC2eRVX0ic&#x26;t=1830</a></td></tr><tr><td align="center">Local Windows Credentials: SAM Dump</td><td align="center">BASTION</td><td align="center">Extracting local passwords from SAM and SYSTEM with secretsdump</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=2j3FNp5pjQ4&#x26;t=660">https://youtube.com/watch?v=2j3FNp5pjQ4&#x26;t=660</a></td></tr><tr><td align="center">Local Windows Credentials: LAPS</td><td align="center">STREAMIO</td><td align="center">Identifying and Extracting the LAPS Password</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=qKcUKlwoGw8&#x26;t=5300">https://youtube.com/watch?v=B9nozi1PrhY&#x26;t=11697</a></td></tr><tr><td align="center">Local Windows Credentials: LAPS</td><td align="center">PivotAPI</td><td align="center">Discovering a user who can add groups to LAPS</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=FbTxPz_GA4o&#x26;t=5990">https://youtube.com/watch?v=FbTxPz_GA4o&#x26;t=5990</a></td></tr><tr><td align="center">Access Tokens: UAC</td><td align="center">ARKHAM</td><td align="center">UAC Bypass</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=krC5j1Ab44I&#x26;t=3340">https://youtube.com/watch?v=krC5j1Ab44I&#x26;t=3340</a></td></tr><tr><td align="center">Access Tokens: SeImpersonate</td><td align="center">SCRAMBLED</td><td align="center">Abusing SeImpersonate Privilege</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=_8FE3JZIPfo&#x26;t=1800">https://youtube.com/watch?v=2j3FNp5pjQ4&#x26;t=660</a></td></tr><tr><td align="center">Access Tokens: Incognito</td><td align="center">HACKBACK</td><td align="center">Using incognito to grab our impersonation token for HACKER user</td><td align="center">Walkthrough : <a href="https://youtube.com/watch?v=B9nozi1PrhY&#x26;t=11697">https://youtube.com/watch?v=B9nozi1PrhY&#x26;t=11697</a></td></tr></tbody></table>
```

</details>

<details>

<summary>Linux Lateral Movement</summary>

8.

```
|    Attack Type    | HTB Machine |                                   Attack Used in HTB                                   |                                                            Link                                                            |
```

```
| :---------------: | :---------: | :------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------: |
|       DevOps      |     SEAL    |                                Abusing ansible playbook                                |  Walkthrough: [https://www.youtube.com/watch?v=wCfztTcioU8\&t=1260s](https://www.youtube.com/watch?v=wCfztTcioU8\&t=1260s) |
|       DevOps      |    INJECT   |                      Ansible enumeration and privilege escalation                      |  Walkthrough: [https://www.youtube.com/watch?v=3VuIaUvHsTI\&t=1320s](https://www.youtube.com/watch?v=3VuIaUvHsTI\&t=1320s) |
| Kerberos on Linux |   TENTACLE  | Configuring our attacker’s box kerberos to connect to Tentacle’s KDC, and steal keytab |      Walkthrough : [https://youtube.com/watch?v=kKhuUXPmJ\_o\&t=3870](https://youtube.com/watch?v=kKhuUXPmJ_o\&t=3870)     |
| Kerberos on Linux |   CERBERUS  |                examine the SSSD configuration and get a domain password                | Walkthrough : [https://www.youtube.com/watch?v=IX4h5aaSK1g\&t=2160s](https://www.youtube.com/watch?v=IX4h5aaSK1g\&t=2160s) |
| Kerberos on Linux |   SEKHMET   |               Dumping the sssd.ldb, Using kinit to get a kerberos ticket               |      Walkthrough : [https://youtube.com/watch?v=kKhuUXPmJ\_o\&t=3870](https://youtube.com/watch?v=vsgPsMZx59w\&t=2400)     |
```

</details>

<details>

<summary>Microsoft SQL Server</summary>

9.

```
<table><thead><tr><th align="center">Attack Type</th><th align="center">HTB Machine</th><th width="198" align="center">Attack Used in HTB</th><th align="center">Link</th></tr></thead><tbody><tr><td align="center">MS SQL in AD</td><td align="center">ESCAPE</td><td align="center">Using mssqlclient to login to access MSSQL</td><td align="center">Walkthrough: <a href="https://youtube.com/watch?v=PS2duvVcjws&#x26;t=390">https://youtube.com/watch?v=PS2duvVcjws&#x26;t=390</a></td></tr><tr><td align="center">MS SQL Escalation</td><td align="center">SCRAMBLED</td><td align="center">enabling xp_cmdshell and getting a reverse shell</td><td align="center">Walkthrough: <a href="https://youtube.com/watch?v=_8FE3JZIPfo&#x26;t=1000">https://youtube.com/watch?v=_8FE3JZIPfo&#x26;t=1000</a></td></tr><tr><td align="center">MS SQL Escalation</td><td align="center">STREAMIO</td><td align="center">Using xp_dirtree to make the MSSQL database connect back to us and steal the hash</td><td align="center">Walkthrough: <a href="https://youtube.com/watch?v=qKcUKlwoGw8&#x26;t=1335">https://youtube.com/watch?v=_8FE3JZIPfo&#x26;t=1000</a></td></tr></tbody></table>
```

</details>

<details>

<summary>Active Diretory Exploitation </summary>

10. |           Attack Type           |  HTB Machine |                                Attack Used in HTB                                |                                                                                                                            Link                                                                                                                            |
    | :-----------------------------: | :----------: | :------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
    |   AD Object permission theory   |     REEL     | Explaining Active Directory (AD) Security Objects (GenericWrite, WriteOwner,etc) |                                                                                                walkthrough: <https://youtube.com/watch?v=ob9SgtFm6_g&t=3205>                                                                                               |
    |        Abusing GenericAll       |    SUPPORT   |                       Abusing GenericAll object permission                       |                                                                                                walkthrough: <https://youtube.com/watch?v=iIveZ-raTTQ&t=970>                                                                                                |
    |  Abusing GenericAll, WriteDACL  |  MULTIMASTER |                           Abusing GenericAll, WriteDACL                          |                                                                                                Walkthrough: <https://youtube.com/watch?v=iwR746pfTEc&t=7410>                                                                                               |
    | Abusing GenericWrite, WriteDACL |     REEL     |                Taking ownership and changing other user’s password               |                                                                                                Walkthrough: <https://youtube.com/watch?v=ob9SgtFm6_g&t=3503>                                                                                               |
    |       Kerberos Delegation       |   PivotAPI   |      Unconstrained delegation with the SQL User. Upload rubeus, use tgtdeleg     |                                                                                                Walkthrough: <https://youtube.com/watch?v=FbTxPz_GA4o&t=7215>                                                                                               |
    |       Kerberos Delegation       | INTELLIGENCE |                             Unconstrained delegation                             |                                                                                     Walkthrough: <https://secnigma.wordpress.com/2021/11/27/hack-the-box-intelligence/>                                                                                    |
    |       Kerberos Delegation       |    SUPPORT   |                                       RBCD                                       | <p>walkthrough: <a href="https://youtube.com/watch?v=iIveZ-raTTQ&#x26;t=970"><https://youtube.com/watch?v=iIveZ-raTTQ&#x26;t=970></a><br>Post: <a href="https://pencer.io/ctf/ctf-htb-support/#rbcd"><https://pencer.io/ctf/ctf-htb-support/#rbcd></a></p> |

    <br>

</details>
