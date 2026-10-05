> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/credentialed-enumeration-from-windows.md).

# Credentialed Enumeration - from Windows

### **Importar o módulo PowerShell do ActiveDirectory**

* **Carregar módulo ActiveDirectory**

```powershell
Import-Module ActiveDirectory
```

* **Descubra Módulos**

```powershell
Get-Module
```

* **Obter informações de domínio**

```powershell
Get-ADDomain
```

* **Obter-ADUser**

```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

* **Verificando relacionamentos de confiança**

```powershell
Get-ADTrust -Filter *
```

* **Enumeração de grupo**

```powershell
Get-ADGroup -Filter * | select name
```

* Informações detalhadas do grupo

```powershell
Get-ADGroup -Identity "Backup Operators"
```

* Associação de grupo

```powershell
Get-ADGroupMember -Identity "Backup Operators"
```

### [PowerView](/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/credentialed-enumeration-from-windows/powerview.md)

* **Importar o PowerView\.ps1**

```powershell
. .\PowerView.ps1
```

* **Informações do usuário do domínio**

```powershell
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```

* **Associação de grupo recursiva**

```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

* **Enumeração de confiança**

```powershell
Get-DomainTrustMapping
```

* **Testando o acesso do administrador local**

```powershell
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```

* **Encontrando usuários com SPN definido**

```powershell
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

### **SharpView**

```powershell
.\SharpView.exe Get-DomainUser -Identity forend
```

### Snaffler

[O Snaffler](https://github.com/SnaffCon/Snaffler) é uma ferramenta que pode nos ajudar a adquirir credenciais ou outros dados confidenciais em um ambiente do Active Directory.&#x20;

* **Execução do Snaffler**

```powershell
.\Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```

### BloodHound

* **SharpHound em ação**

```powershell
.\SharpHound.exe -c All --zipfilename ILFREIGHT
```
