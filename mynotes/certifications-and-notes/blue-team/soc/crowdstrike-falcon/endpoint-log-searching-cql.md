> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/endpoint-log-searching-cql.md).

# Endpoint Log Searching / CQL

É a linguagem que você usa para pesquisar logs brutos (raw logs) dos endpoints dentro do Falcon

> “SIEM > Advanced Event Search = Raw Logs”

Ou seja:

* Você está vendo **logs crus do endpoint**
* Igual um **mini-SIEM dentro do Falcon**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FK4uNOAFDYV7s7kdnEdSL%2Fimage.png?alt=media&amp;token=143f40d5-d9be-4187-9aa0-d9bcac857a93" alt=""><figcaption></figcaption></figure>

**Campos MAIS importantes**

* `event_simpleName`
* `@timestamp`
* `CommandLine`
* `UserName`
* `ComputerName`
* `FileName`
* `ImageFileName`
* `SHA256HashData`
* `DomainName`
* `LocalIP / RemoteIP`

**O que você procura**&#x20;

**CommandLine**&#x20;

Campo mais importante

&#x20;      Procure:

* `powershell -enc`
* `cmd /c`
* downloads:
  * `Invoke-WebRequest`
  * `curl`
* comandos longos/obfuscados

**UserName**

&#x20;     Procure:

* Admin executando coisa estranha
* Conta de serviço rodando script
* Usuário comum fazendo ação avançada

**ImageFileName / FileName**

&#x20;     Procure:

* LOLBins:
  * `powershell.exe`
  * `cmd.exe`
  * `rundll32.exe`
* Arquivos em:
  * `AppData`
  * `Temp`

**IP / Domain**

Procure:

* Conexões externas
* IP desconhecido
* Domínio recente

**Hash (SHA256)**

Joga no:

* VirusTotal

#### Queries úteis&#x20;

* **Buscar PowerShell suspeito**

```
event_simpleName=ProcessRollup2 
ImageFileName="*powershell.exe*"
```

* **Buscar comandos com base64**

```
CommandLine="*-enc*"
```

* **Buscar execução em Temp**

```
CommandLine="*AppData*"
```

* **Buscar conexões de rede**

```
event_simpleName=NetworkConnectIP4
```

**Base Search**

Esse aqui é ESSENCIAL:

```
| time := formatTime("%Y-%m-%d %H:%M:%S", field=@timestamp, locale=en_US, timezone=Z)
| select([time,UserName,ImageFileName,CommandLine,ParentBaseFileName,SHA256HashData])
```

Isso transforma log cru em algo legível.

**Agrupamento (descobrir padrões)**

```
groupBy([DomainName], function=[count(as=count)])
| sort([count], order=desc)
```

Serve para:

* Ver domínio mais acessado
* Detectar beaconing ou C2

**Wildcard search**

```
/stringsuspeita/i
```

Busca flexível

**Caminhos estranhos**

> “\Device\HarddiskVolume# → C:\”

Sempre converte mentalmente

**Tempo**

* Sempre usar `@timestamp`
* Converter para UTC

#### Como usar isso na triagem

**Fluxo real:**

1. Você vê um alerta
2. Vai no CQL
3. Procura:
   * Mesmo processo
   * Mesmo usuário
   * Mesmo hash
4. Expande:
   * Outros hosts?
   * Mesmo domínio/IP?
