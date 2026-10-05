> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/machines-to-pratice-for/cpts-preparation.md).

# CPTS Preparation

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FvUpXMNH3AveZBf2Gzrgo%2Fimage.png?alt=media&amp;token=dfa05a3f-de19-459c-8816-5637ef7b1b05" alt=""><figcaption></figcaption></figure>

The Hack The Box CPTS Preparation track equips learners with the essential knowledge and practical skills needed to succeed in the Certified Penetration Testing Specialist exam.

— [Hack The Box](https://app.hackthebox.com/tracks/CPTS-Preparation)

#### EASY

<table><thead><tr><th>Attack Type</th><th>HTB Machine</th><th width="219.4000244140625">Attack Used in HTB</th><th>Walkthrough </th><th>Tool / Notes</th></tr></thead><tbody><tr><td>AD/Certificate abuse, WinRM</td><td><strong>Fluffy</strong> </td><td>Initial exploit → credential access → abuse of AD/ADCS and WinRM for lateral/privilege escalation.</td><td><a href="https://www.youtube.com/watch?v=KvUC7bakm-E">Fluffy — walkthrough </a></td><td>certipy, WinRM, RCE PoC</td></tr><tr><td>Web: SQLi / LFI / log poisoning</td><td><strong>Trick</strong> </td><td>SQL injection → LFI / log poisoning → escalate through service misconfigurations.</td><td><a href="https://www.youtube.com/watch?v=ai98umjeO8M&#x26;t=1150s">Trick — walkthrough </a></td><td>sqlmap, LFI/log-poisoning techniques</td></tr><tr><td>Service misconfiguration (Redis / SSH key)</td><td><strong>Postman</strong> </td><td>Unauthenticated Redis / write SSH key to get initial access; follow-up privilege escalation.</td><td><a href="https://www.youtube.com/watch?v=jJnHET1o8ZQ">Postman — walkthrough  </a></td><td>redis-cli, ssh-key exploitation, john/hashcat</td></tr></tbody></table>

#### MEDIUM

#### AD / Domain-focused (Medium)

| Attack Type                                   | HTB Machine       | Attack Used in HTB                                                                                       | Walkthrough                                                                       | Tool / Notes                                      |
| --------------------------------------------- | ----------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------- |
| Web/Service → Jenkins RCE → PrivEsc (Windows) | **Jeeves**        | Abuse of Jenkins (unauthenticated/script console) for RCE, then Windows privilege escalation techniques. | [Jeeves — walkthrough](https://www.youtube.com/watch?v=EKGBskG8APc)               | Jenkins console, post-exploitation priv‑esc tools |
| ADCS / Certificate / AD enumeration           | **TombWatcher**   | AD enumeration + ADCS abuse (certificate-related escalation techniques).                                 | [TombWatcher — walkthrough](https://www.youtube.com/watch?v=um8b-TN76bY)          | certipy, BloodHound, impacket                     |
| Kerberos / relay / credential discovery       | **VulnCicada**    | Information leak + Kerberos relay / certificate relay to obtain machine certs or domain access.          | [VulnCicada — walkthrough](https://0xdf.gitlab.io/2025/07/03/htb-vulncicada.html) | petitpotam, certipy, impacket                     |
| Kerberoast / DPAPI / secret extraction        | **Voleur**        | Kerberoast / targeted ticket cracking + DPAPI / secrets dump to recover creds and escalate.              | [Voleur — walkthrough ](https://www.hyhforever.top/posts/2025/07/htb-voleur/)     | Rubeus, mimikatz, secretsdump                     |
| AD: ACL / GenericAll / password reset abuse   | **Administrator** | AD ACL enumeration and abuse (GenericAll / password resets) to gain domain admin.                        | [Administrator — walkthrough](https://www.youtube.com/watch?v=Miam4nw9pmE)        | BloodHound, impacket, aclpwn-style techniques     |
| Ansible / leaked vault blobs → AD / creds     | **Authority**     | Discovery of Ansible vault blobs / secrets → decrypt/crack to obtain creds and pivot in the network.     | [Authority — walkthrough ](https://www.youtube.com/watch?v=7AF5riqLy-8)           | ansible-vault, john/hashcat, AD tooling           |

#### Web / Application / Service (Medium)

| Attack Type                                       | HTB Machine  | Attack Used in HTB                                                                                           | Walkthrough                                                           | Tool / Notes                            |
| ------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- | --------------------------------------- |
| ASP.NET insecure deserialization / ViewState      | **Pov**      | Information disclosure (web.config), insecure deserialization (ViewState/ysoserial) → RCE and escalation.    | [Pov — walkthrough](https://www.youtube.com/watch?v=84xCsHvkxYE)      | ysoserial.net, PowerShell, mimikatz     |
| Web: subdomain enum → SQLi → LFI/RFI              | **StreamIO** | Subdomain enumeration → SQL injection to steal creds → LFI/RFI to achieve RCE.                               | [StreamIO — walkthrough](https://www.youtube.com/watch?v=qKcUKlwoGw8) | sqlmap, LFI → RFI chains                |
| Web upload / symlink privesc / file abuse         | **Media**    | Malicious upload / file handling to obtain credentials or hashes; symlink/FS abuse for privilege escalation. | Media — walkthrough                                                   | file upload exploitation, hash cracking |
| Web app / Git / Vault interplay → host compromise | **Craft**    | Exploitation of web services (Gogs/Vault or similar) → steal secrets, pivot to host and escalate.            | [Craft — walkthrough](https://www.youtube.com/watch?v=3znkLWakuUA)    | web RCE, Vault access, SSH pivot        |

#### HARD

| Attack Type                                          | HTB Machine    | Attack Used in HTB                                                                                                                    | Walkthrough                                                                       | Tool / Notes                             |
| ---------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------- |
| AD / delegation (FTP / KeePass → MSSQL → delegation) | **Redelegate** | Anonymous FTP → KeePass database recovery → MSSQL credentials → abuse of constrained delegation / force-change to escalate to domain. | [Redelegate — walkthrough](https://0xdf.gitlab.io/2025/07/17/htb-redelegate.html) | keepass2john/john, mssql tools, impacket |
| Complex LFI → DNS/key leak → interception → priv esc | **Snoopy**     | LFI to read BIND/DNS keys → manipulate DNS / intercept password resets (mail/Mattermost) → SSH MITM/privilege escalation.             | [Snoopy — walkthrough](https://www.youtube.com/watch?v=6tn30O0SjVQ)               | socat/mitmproxy, SSH MITM, mail spoofing |

#### INSANE

| Attack Type                                                        | HTB Machine | Attack Used in HTB                                                                                                            | Walkthrough                                                        | Tool / Notes                                                            |
| ------------------------------------------------------------------ | ----------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| Multi-step AD / LDAP injection / container escape / AD trust abuse | **Ghost**   | LDAP injection → credential exfiltration (Gitea) → RCE → container pivot → abuse of AD trusts and multi-step domain takeover. | [Ghost — walkthrough](https://www.youtube.com/watch?v=4dEmocjKnZg) | LDAP injection scripts, container escape payloads, impacket, AD tooling |
