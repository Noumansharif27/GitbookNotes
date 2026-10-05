> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/real-time-response.md).

# Real Time Response

**RTR** é um **acesso remoto via linha de comando (CLI)** ao endpoint.

{% embed url="<https://www.crowdstrike.com/tech-hub/endpoint-security/the-power-of-real-time-response/>" %}

Ou seja:

> Você consegue **investigar e agir diretamente na máquina** sem precisar acessar fisicamente ou via RDP.

**Explicando de forma simples**

RTR = tipo um:

* SSH (Linux)
* PowerShell remoto (Windows)

Mas integrado ao Falcon e focado em **resposta a incidente**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FKw9hflK8W0PKujML3cIX%2Fimage.png?alt=media&amp;token=00670fe1-01ed-4aab-b3d1-c290260a8819" alt=""><figcaption></figcaption></figure>

**O que você pode fazer com RTR**

**Investigação**

* Listar processos → `ps`
* Ver conexões → `netstat`
* Navegar arquivos → `ls`, `cd`
* Ler arquivos → `cat`

**Resposta (ação direta)**

* Matar processo → `kill`
* Coletar arquivo → `get`
* Executar script → `runscript`
* Rodar comando → `run`

**Coleta forense**

* Baixar arquivos suspeitos
* Coletar logs
* Analisar artefatos

> arquivos baixados vêm zipados com senha “infected”

**Onde acessar**

Você pode abrir RTR a partir de:

* Host Management
* Detections
* Incidents
* Event Actions

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F4DZNnoPKzwpvJh5RNjzz%2Fimage.png?alt=media&amp;token=a303891b-d98a-4bdc-88b7-f0d313ec3a96" alt=""><figcaption></figcaption></figure>

**Condição IMPORTANTE**

&#x20;O host precisa estar:

* **Online**
* Conectado ao Falcon

> “Online = powered on and connected to the cloud console”

**Comandos básicos**

* `ps` → processos
* `netstat` → conexões
* `ls` → listar arquivos
* `cd` → navegar
* `cat` → ler arquivo
* `kill` → matar processo
* `get` → baixar arquivo
* `put` → enviar arquivo
* `run` → executar comando
* `runscript` → rodar script

#### O que procurar quando estiver usando RTR

**Processos suspeitos**

* PowerShell rodando
* Processo desconhecido
* Nome estranho

**Conexões ativas**

* IP externo desconhecido
* Porta suspeita

**Arquivos maliciosos**

* Em:
  * `AppData`
  * `Temp`
* Executáveis estranhos

#### Quando usar RTR

Use quando:

* &#x20;Precisa confirmar atividade no host
* Quer coletar evidência
* Precisa conter ameaça
* Quer validar suspeita do alerta
