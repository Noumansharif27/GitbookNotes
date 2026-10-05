> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/credentialed-enumeration-from-windows/powerview.md).

# PowerView

[O PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon) é uma ferramenta escrita em PowerShell para nos ajudar a obter consciência situacional dentro de um ambiente AD. Assim como o BloodHound, ele fornece uma maneira de identificar onde os usuários estão logados em uma rede, enumerar informações de domínio como usuários, computadores, grupos, ACLS, trusts, procurar compartilhamentos de arquivos e senhas, executar Kerberoasting e muito mais.

| **Comando**           | **Descrição**                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------ |
| `Export-PowerViewCSV` | Anexar resultados a um arquivo CSV                                                         |
| `ConvertTo-SID`       | Converter um nome de usuário ou grupo em seu valor SID                                     |
| `Get-DomainSPNTicket` | Solicita o tíquete Kerberos para uma conta de Nome Principal de Serviço (SPN) especificada |

### **Funções de domínio/LDAP:**

| `Get-Domain`                | Retornará o objeto AD para o domínio atual (ou especificado)                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------- |
| `Get-DomainController`      | Retorna uma lista dos Controladores de Domínio para o domínio especificado                              |
| `Get-DomainUser`            | Retornará todos os usuários ou objetos de usuários específicos no AD                                    |
| `Get-DomainComputer`        | Retornará todos os computadores ou objetos de computador específicos no AD                              |
| `Get-DomainGroup`           | Retornará todos os grupos ou objetos de grupo específicos no AD                                         |
| `Get-DomainOU`              | Pesquisar todos ou objetos UO específicos no AD                                                         |
| `Find-InterestingDomainAcl` | Encontra ACLs de objetos no domínio com direitos de modificação definidos para objetos não incorporados |
| `Get-DomainGroupMember`     | Retornará os membros de um grupo de domínio específico                                                  |
| `Get-DomainFileServer`      | Retorna uma lista de servidores que provavelmente funcionam como servidores de arquivos                 |
| `Get-DomainDFSShare`        | Retorna uma lista de todos os sistemas de arquivos distribuídos para o domínio atual (ou especificado)  |

### **Funções do GPO:**

| `Get-DomainGPO`    | Retornará todos os GPOs ou objetos GPO específicos no AD                                          |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| `Get-DomainPolicy` | Retorna a política de domínio padrão ou a política do controlador de domínio para o domínio atual |

### **Funções de enumeração de computador:**

| `Get-NetLocalGroup`       | Enumera grupos locais na máquina local ou remota                               |
| ------------------------- | ------------------------------------------------------------------------------ |
| `Get-NetLocalGroupMember` | Enumera membros de um grupo local específico                                   |
| `Get-NetShare`            | Retorna compartilhamentos abertos na máquina local (ou remota)                 |
| `Get-NetSession`          | Retornará informações de sessão para a máquina local (ou remota)               |
| `Test-AdminAccess`        | Testa se o usuário atual tem acesso administrativo à máquina local (ou remota) |

### **Funções 'Meta' encadeadas:**

| `Find-DomainUserLocation`         | Encontra máquinas onde usuários específicos estão logados                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `Find-DomainShare`                | Encontra compartilhamentos acessíveis em máquinas de domínio                                            |
| `Find-InterestingDomainShareFile` | Pesquisa por arquivos que correspondem a critérios específicos em compartilhamentos legíveis no domínio |
| `Find-LocalAdminAccess`           | Encontre máquinas no domínio local onde o usuário atual tem acesso de administrador local               |

### **Funções de confiança de domínio:**

| `Get-DomainTrust`              | Retorna trusts de domínio para o domínio atual ou um domínio especificado              |
| ------------------------------ | -------------------------------------------------------------------------------------- |
| `Get-ForestTrust`              | Retorna todos os trusts de floresta para a floresta atual ou uma floresta especificada |
| `Get-DomainForeignUser`        | Enumera usuários que estão em grupos fora do domínio do usuário                        |
| `Get-DomainForeignGroupMember` | Enumera grupos com usuários fora do domínio do grupo e retorna cada membro estrangeiro |
| `Get-DomainTrustMapping`       | Enumerará todos os trusts para o domínio atual e quaisquer outros vistos.              |
