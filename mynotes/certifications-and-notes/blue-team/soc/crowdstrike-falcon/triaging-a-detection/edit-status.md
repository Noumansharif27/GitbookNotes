> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/triaging-a-detection/edit-status.md).

# Edit Status

#### **Edit Status**

É onde você marca o estado da detecção:

* Se é incidente real
* Se é falso positivo
* Se ainda está em investigação

Ou seja:

> é a sua **decisão final (ou parcial)** sobre o alerta

#### Status mais comuns e quando usar

**New / Open**

* Estado inicial
* Ainda não analisado

Use quando:

* Acabou de chegar alerta

**In Progress**

* Você está investigando

Use quando:

* Ainda está analisando
* Precisa de mais dados

**True Positive**

**Confirmou atividade maliciosa**

Use quando:

* Há evidência clara de ataque
* Ex: execução suspeita + comportamento malicioso

Normalmente:

* Vai escalar para IR (Incident Response)

**False Positive**

Alerta legítimo, mas comportamento é benigno

&#x20;Exemplos:

* Script de admin
* Ferramenta interna
* Automação da empresa

Cuidado:

* Sempre justificar bem

**Benign True Positive**

Muito importante (muita gente erra aqui)

* O comportamento aconteceu
* Mas **é legítimo**

Ex:

* Admin rodando PowerShell
* Ferramenta de deploy

Diferença:

* **True Positive → ataque**
* **Benign True Positive → comportamento esperado**

**Ignored / Closed**

* Encerrado sem ação relevante

**O que você DEVE escrever ao editar status**

Nunca só muda o status — sempre documenta:

Inclua:

* O que foi analisado
* Por que é benigno ou malicioso
* Evidências (processo, command line, usuário)

**Exemplo prático**

**False Positive**

> Activity triggered by internal IT script.\
> PowerShell execution confirmed as part of approved automation.\
> No malicious indicators found.

**True Positive**

> Suspicious PowerShell execution with encoded command.\
> Spawned from Word document, indicating possible macro-based attack.\
> Requires escalation.

**Erros comuns (evita isso)**

* Marcar como False Positive sem investigar
* Não olhar process tree
* Não justificar decisão<br>

&#x20;Confundir:

* False Positive&#x20;
* Benign True Positive&#x20;
