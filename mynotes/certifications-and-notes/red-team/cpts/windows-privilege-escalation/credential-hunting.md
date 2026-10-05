> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/credential-hunting.md).

# Credential Hunting

**Procurando arquivos**

```ps
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml
```

* Informações confidenciais do IIS, como credenciais, podem ser armazenadas em um arquivo `web.config`&#x20;

```
C:\inetpub\wwwroot\web.config
```

* **Chrome Dictionary Files**

```ps
gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password
```

* **Arquivo de histórico do PowerShell**

```ps
gc (Get-PSReadLineOption).HistorySavePath
```

* Também podemos usar este comando simples para recuperar o conteúdo de todos os arquivos de histórico do PowerShell aos quais temos acesso como usuário atual

```ps
foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}
```
