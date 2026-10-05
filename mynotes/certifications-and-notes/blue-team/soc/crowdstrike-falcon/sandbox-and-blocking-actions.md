> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/sandbox-and-blocking-actions.md).

# Sandbox & Blocking Actions

#### **Sandbox**

Um sandbox é um ambiente isolado e controlado usado para executar arquivos, links ou código suspeito sem risco para o sistema real.

* Existe limite no Crowdstrike Falcon:\
  \~100 arquivos por mês
* Arquivos já **quarentenados pelo Falcon NÃO contam**

#### Para que serve

Executar arquivos suspeitos em ambiente isolado:

&#x20;     Ver:

* Comportamento
* Rede
* Persistência
* MITRE ATT\&CK

#### Quando usar (SOC)

Use quando:

* Hash desconhecido
* Arquivo suspeito
* Precisa confirmar comportamento

#### O que procurar na análise

* Conexão externa (C2)
* Criação de processos
* Escrita em registry
* Download de payload
* Obfuscação

#### **Blocking Actions**&#x20;

**Onde fazer bloqueios**

Página de **IOC Management**

**Tipos de ação**

&#x20;     **Block**

* Bloqueia execução

&#x20;      **Detect Only**

* Só alerta (não bloqueia)

&#x20;       **Allow**

* Permite (whitelist)

&#x20;       **Block and don’t show detection**

* Bloqueia silenciosamente

**Limitação CRÍTICA**&#x20;

> Só dá pra bloquear **hash de arquivo executável (.exe)**

Para IP e domínio:

* Não bloqueia aqui

#### Então como bloquear IP/Domínio?

**HBFW (Host-Based Firewall)**

&#x20;     Tipo IPS

* Bloqueia IPs
* Bloqueia tráfego

#### **Custom IOAs**

Regras customizadas

Pode detectar/bloquear:

* IP
* Domínio
* Processos
* Arquivos
* Conexões de rede

**Custom IOAs**&#x20;

&#x20;      Sempre:

* Testar regex
* Validar regra antes de aplicar
