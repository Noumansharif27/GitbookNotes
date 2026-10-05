> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/initial-enumeration.md).

# Initial Enumeration

* **Lista de tarefas**

```powershell
tasklist /svc
```

* **Exibir todas as variáveis ​​de ambiente**

```powershell
set
```

* **Veja as informações detalhadas de configuração.**

```powershell
systeminfo
```

* **Correções e atualizações**

```powershell
wmic qfe
```

* Também podemos fazer isso com o PowerShell usando o cmdlet [Get-Hotfix](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-hotfix?view=powershell-7.1)&#x20;
* Pesquise no Google os KBs instalados em [Hotfixes](https://www.catalog.update.microsoft.com/Search.aspx?q=hotfix) para ter uma ideia de quando o servidor foi atualizado.&#x20;

```powershell
Get-HotFix | ft -AutoSize
```

* **Programas instalados**

```
wmic product get name
```

* Também podemos verificar programas instalados, usando o cmdlet [Get-WmiObject ](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) do powershell

```powershell
Get-WmiObject -Class Win32_Product |  select Name, Version
```

* **Exibir processos em execução**

```powershell
netstat -ano
```

* **Usuários conectados**

```
query user
```

* **Usuário atual**
* Ao obtermos acesso a um host, devemos sempre verificar primeiro em qual contexto de usuário nossa conta está sendo executada.&#x20;

```powershell
echo %USERNAME%
```

* **Privilégios do usuário atual**

```powershell
whoami /priv
```

* **Obter todos os usuários**

```
net user
```

* **Detalhes sobre um grupo**

```powershell
net localgroup administrators
```

* **Consulte a política de senhas e outras informações da conta.**

```powershell
net accounts
```

* Existem muitos guias de referência disponíveis para nos ajudar, como [este](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md) e [este](https://github.com/dollarboysushil/oscp-cpts-notes/tree/main/windows-privilege-escalation)
