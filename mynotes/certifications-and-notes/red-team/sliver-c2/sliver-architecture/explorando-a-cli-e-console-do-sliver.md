> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/sliver-architecture/explorando-a-cli-e-console-do-sliver.md).

# Explorando a CLI e Console do Sliver

O **Sliver C2** é gerenciado via CLI (linha de comando), permitindo:

* Criar implantes
* Gerenciar conexões
* Configurar listeners
* Interagir com máquinas comprometidas

**Acessando o console**

* Abra o terminal no Kali Linux
* Execute:

```
sliver
```

* Se der erro, inicie o serviço:

```
sudo systemctl start sliver
```

**Comando principal: help**

* `help` → lista todos os comandos disponíveis
* `help <command>` → mostra detalhes e exemplos

&#x20;É o principal recurso para aprender a CLI.

**Comandos importantes**

**help**

* Lista todos os comandos
* Base para explorar o framework

**generate**

* Cria implantes (payloads)
* Define protocolo C2 (mtls, http, dns, wireguard, etc.)
* Permite configurar:
  * Sistema operacional (`--os`)
  * Arquitetura (`--arch`)
  * Local de saída (`--save`)

**mtls / http / https / dns / wireguard**

* Iniciam **listeners**
* Listener = servidor que recebe conexão do implante
* Cada comando corresponde a um protocolo

**sessions**

* Gerencia conexões ativas
* Funções:
  * Listar sessões
  * Interagir (`-i`)
  * Encerrar (`-k`)

**use**

* Acessa uma sessão ativa

```
use <session_id>
```

* Abre interação direta com o alvo

**background**

* Sai da sessão atual
* Mantém conexão ativa em segundo plano

**websites**

* Gerencia conteúdo hospedado no C2
* Pode servir:
  * Sites falsos (decoy)
  * Payloads web

**Resumo geral**

* A CLI do Sliver é totalmente baseada em comandos
* `help` é essencial para navegação
* O fluxo básico:
  1. Criar listener
  2. Gerar implante
  3. Receber sessão
  4. Interagir com o alvo
