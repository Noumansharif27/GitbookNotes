> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/osint-tools-to-leverage.md).

# OSINT Tools to Leverage

#### **Análise de arquivos (hash / malware)**

**VirusTotal**

{% embed url="<https://www.virustotal.com/gui/home/upload>" %}

Use para:

* Hash (MD5, SHA256)
* IP / domínio
* Arquivo

&#x20;      Procure:

* Quantos vendors detectam
* Nome da ameaça
* Behavior (aba Behavior)

Dica:

> 1–2 detecções = pode ser falso positivo\
> 10+ = suspeito\
> 30+ = provavelmente malicioso

#### Hybrid Analysis

Sandbox com análise profunda

Procure:

* Comportamento (registry, network)
* MITRE ATT\&CK
* Droppers

**ANY.RUN & CrowdStrike Falcon Sandbox**

{% embed url="<https://app.any.run/>" %}

{% embed url="<https://www.crowdstrike.com/en-us/platform/threat-intelligence/malware-analysis/?utm_campaign=brand&utm_content=crwd-brand-amer-bra-en-psp-x-x-x-tct-x_x_x_threatih-x&utm_medium=sem&utm_source=goog&utm_term=crowdstrike%20malware%20sandboxing&utm_language=en-us&cq_cmp=22044871114&cq_plac={placement]&gad_source=1&gad_campaignid=22044871114&gbraid=0AAAAAC-K3YRBwnLLSSdJxcKhQfTyKGKPw&gclid=EAIaIQobChMIypTo1Z-0kwMVc1VIAB3TCxTZEAAYASAAEgLNxvD_BwE>" %}

Sandbox interativa (muito usada em SOC)

Diferencial:

* Você vê execução ao vivo
* Timeline visual

&#x20;Procure:

* Conexões externas
* Spawn de processos
* Download de payloads

**Joe Sandbox**

{% embed url="<https://www.joesandbox.com/#windows>" %}

Relatórios muito detalhados

Bom para:

* Análise mais técnica (nível L2/L3)

#### **Análise de URLs e domínios**

**urlscan.io**

{% embed url="<https://urlscan.io/>" %}

Analisa páginas web

Procure:

* Redirecionamentos
* Scripts suspeitos
* Domínios relacionados

**DomainTools (WHOIS)**

{% embed url="<https://who.is/>" %}

Inteligência de domínio

Procure:

* Data de criação
* Registrante
* Histórico

#### **Infraestrutura / exposição**

**Shodan**

{% embed url="<https://www.shodan.io/>" %}

“Google da internet exposta”

Use para:

* IPs suspeitos
* Portas abertas
* Serviços expostos

&#x20;     Procure:

* RDP aberto
* Serviços incomuns

**MXToolbox**

{% embed url="<https://mxtoolbox.com/>" %}

Focado em DNS e e-mail

Use para:

* MX records
* SPF / DKIM
* Blacklists

#### Como usar isso na triagem (fluxo real)

**Exemplo prático**

Você pega no Falcon:

* Hash
* IP
* Domínio

&#x20;Faz:

1. **Hash → VirusTotal**
2. **Se suspeito → ANY.RUN / Hybrid Analysis**
3. **Domínio → urlscan + DomainTools**
4. **IP → Shodan**
5. **Email/domain → MXToolbox**
