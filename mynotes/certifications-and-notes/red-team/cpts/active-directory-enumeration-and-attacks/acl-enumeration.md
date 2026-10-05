> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/acl-enumeration.md).

# ACL Enumeration

### Enumerando ACLs com o PowerView

* Importar o módulo

```powershell
. .\PowerView.ps1

$sid = Convert-NameToSid wley #Obter o SID de um user
```

* **Usando Get-DomainObjectACL**

```powershell
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```

* **Executando uma pesquisa reversa e mapeando para um valor GUID**

{% code lineNumbers="true" %}

```powershell
$guid= "00299570-246d-11d0-a768-00aa006e0529"
$searchBase = "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)"
Get-ADObject -SearchBase $searchBase -LDAPFilter "(objectClass=ControlAccessRight)" -Properties * | Where-Object {$_.rightsGuid -eq $guid} | Select Name, DisplayName, DistinguishedName, rightsGuid | Format-List
```

{% endcode %}

* **Usando o sinalizador -ResolveGUIDs**

```powershell
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} 
```

**Criando uma lista de usuários do domínio**

```powershell
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```

**Um loop foreach útil**

```powershell
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```

**Enumeração adicional de direitos usando damundsen**

```powershell
$sid2 = Convert-NameToSid damundsen #Alterar sempre o user
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```

**Investigando o grupo de nível 1 do suporte técnico com Get-DomainGroup**

```powershell
Get-DomainGroup -Identity "Help Desk Level 1" | select memberof
```

**Investigando o Grupo de Tecnologia da Informação**

```powershell
$itgroupsid = Convert-NameToSid "Information Technology"
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose
```

**Procurando acesso interessante**

```powershell
$adunnsid = Convert-NameToSid adunn 
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $adunnsid} -Verbose
```
