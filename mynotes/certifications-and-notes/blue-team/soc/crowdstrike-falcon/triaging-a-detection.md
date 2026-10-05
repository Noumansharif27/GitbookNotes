> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/triaging-a-detection.md).

# Triaging a Detection

* As detecções estão presentes para um período de retenção de 90 dias.

* Os indicadores estão presentes com base no seu período de retenção do CS, que provavelmente é de 1 ano.

* Podemos adicionar e remover filtros

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZzwWavRRmAUFXML7p0mC%2Fimage.png?alt=media&amp;token=8071d124-883a-45da-abb0-52e82063211c" alt=""><figcaption></figcaption></figure>

#### Detalhes sobre cada aba

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FYNjlHnhIoM8q1YaYF0S0%2Fimage.png?alt=media&amp;token=d1154a96-d7f9-40fd-aa93-a0a40f82607b" alt=""><figcaption></figcaption></figure>

#### **Details (primeiro filtro mental)**

Aqui você decide se vale investigar mais.

&#x20;**Sinais suspeitos:**

* Severity **High/Critical**
* Técnica MITRE ligada a:
  * Execution
  * Persistence
  * Lateral Movement
* Processo estranho (ex: `powershell.exe`, `rundll32.exe`, `wscript.exe`)
* Usuário fora do padrão (admin executando coisa incomum)
* Horário estranho (madrugada, fora do expediente)

&#x20;Pergunta-chave:

> Isso faz sentido para esse usuário/host?

#### **Process Tree**&#x20;

Essa é a mais importante.

&#x20;**Procure por:**

**Cadeias estranhas**

Exemplo clássico:

* `explorer.exe → cmd.exe → powershell.exe`
* `winword.exe → powershell.exe`&#x20;

**LOLBins (ferramentas legítimas usadas maliciosamente)**

Fique atento a:

* `powershell.exe`
* `cmd.exe`
* `rundll32.exe`
* `mshta.exe`
* `regsvr32.exe`
* `wmic.exe`

**Processos iniciados por documentos**

Muito suspeito:

* `excel.exe → cmd.exe`
* `acrobat.exe → powershell.exe`

**Filhos inesperados**

Exemplo:

* Navegador iniciando script shell
* Serviço do sistema iniciando script de usuário

#### **Process Table**

Mais visão geral.

Procure:

* Mesmo processo repetido várias vezes
* Execuções com caminhos estranhos:
  * `C:\Users\Public\`
  * `AppData\Temp`
* Binários com nomes parecidos com legítimos:
  * `svch0st.exe` (zero no lugar de “o”)

#### **Process Graph**

Use para confirmar padrões visuais.

Procure:

* Ramificações grandes (muitos processos derivados)
* Comportamento em cadeia (execução em cascata)

#### **Events Timeline**

Aqui você entende a história.

Sinais:

* Execuções muito rápidas em sequência (script automatizado)
* Sequência típica de ataque:
  1. Download
  2. Execução
  3. Persistência

#### **Asset Graph**

Expansão do incidente.

Procure:

* Mesmo hash em múltiplos hosts
* Mesmo usuário em vários endpoints
* Conexões entre máquinas (lateral movement)
