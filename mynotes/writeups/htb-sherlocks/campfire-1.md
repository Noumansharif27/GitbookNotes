> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/htb-sherlocks/campfire-1.md).

# Campfire-1

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F8XVCzNBFKGedvsYJCBq0%2Fimage.png?alt=media&amp;token=867c9088-acba-4ef1-b0c1-223286ddb357" alt="" width="375"><figcaption></figcaption></figure>

**Classificação**:  Very Easy

**Categoria:** DFIR

**Cenário:**

> Alonzo encontrou arquivos estranhos em seu computador e informou a recém-formada equipe do SOC. Avaliando a situação, acredita-se que um ataque Kerberoasting possa ter ocorrido na rede. Sua tarefa é confirmar as descobertas analisando as evidências fornecidas.

#### Questões

1- Analisando os logs de segurança do controlador de domínio, você pode confirmar a data e hora UTC em que ocorreu a atividade de kerberoasting?

* Para identificar quando ocorreu a atividade de **Kerberoasting**, analisei o arquivo `SECURITY-DC.evtx`.\
  Após extrair o log com o **EvtxECmd**:

```
PS C:\Users\johndoe\Desktop\Get-ZimmermanTools\net6\EvtxeCmd> .\EvtxECmd.exe -f "C:\Users\johndoe\Downloads\Triage\Domain Controller\SECURITY-DC.evtx" --csv C:\Users\johndoe\ --csvf dc.csv
```

* Carreguei o arquivo no **Timeline Explorer** e filtrei os eventos **ID 4769**. Procurei especificamente por solicitações TGS usando **Ticket Encryption Type 0x17 (RC4)**, típico de Kerberoasting.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FNXUBnnkCOTafTwO6OYF1%2Fimage.png?alt=media&amp;token=50572412-d241-4431-a6ae-82b5fdcbb879" alt=""><figcaption></figcaption></figure>

* &#x20;A atividade de Kerberoasting ocorreu em: `2024-05-21 03:18:09`

***

**2- Qual o nome do serviço que foi alvo do ataque?**

* Dentro do evento, o campo **Service Name** (ou **Service Principal Name – SPN**) indica qual serviço estava sendo alvo do ataque.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FoQnLfaYcqc3DVbaMw1p6%2Fimage.png?alt=media&amp;token=dd01b5ef-e30b-4878-8d03-56d1f6388a00" alt=""><figcaption></figcaption></figure>

* O serviço alvo do ataque foi:`MSSQLService`

***

**3- É muito importante identificar a estação de trabalho a partir da qual essa atividade ocorreu. Qual é o endereço IP da estação de trabalho?**

* O campo Remote Host registra o endereço IP da máquina que solicitou o Ticket Granting Service (TGS).

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FKjGAZrBZGWAJP45BtsjM%2Fimage.png?alt=media&amp;token=2489cdac-dd73-46d3-81a1-d6cf9f766a06" alt=""><figcaption></figcaption></figure>

* O endereço IP da estação de trabalho responsável pela atividade foi: `172.17.79.129`

***

**4- Agora que identificamos a estação de trabalho, fornecemos uma triagem incluindo logs do PowerShell e arquivos de pré-busca para obter informações mais detalhadas e entender como essa atividade ocorreu no endpoint. Qual é o nome do arquivo usado para enumerar objetos do Active Directory e possivelmente encontrar contas Kerberos na rede?**

* Após identificar a estação de trabalho envolvida, analisei os **logs de PowerShell operacionais**, focando especificamente no **Event ID 4104**, que registra o conteúdo completo de scripts executados.
* Primeiro, extraí os logs para CSV usando o EvtxECmd:

```
PS C:\Users\johndoe\Desktop\Get-ZimmermanTools\net6\EvtxeCmd> .\EvtxECmd.exe -f "C:\Users\johndoe\Downloads\Triage\Workstation\Powershell-Operational.evtx" --csv C:\Users\johndoe\Downloads\ --csvf ps.csv
```

Em seguida, carreguei o arquivo `ps.csv` no **Timeline Explorer** e filtrei por:

* **EventID = 4104**

Esses eventos revelam:

* o **script executado**,
* o **conteúdo do código**,
* e **o nome do arquivo carregado pelo PowerShell**.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FwhRNPM1oVHFTPrsFCKpp%2Fimage.png?alt=media&amp;token=8c6b1a08-aa78-4b85-b9fa-eba10c3ff464" alt=""><figcaption></figcaption></figure>

O arquivo utilizado para enumerar objetos do AD foi: `powerview.ps1`&#x20;

***

**5- Quando este script foi executado? (UTC)**

* O campo **TimeCreated** desse evento indica a data e hora exata em que o script foi carregado e executado no endpoint.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F6HF0QWhJzilF1zDbWmXB%2Fimage.png?alt=media&amp;token=ab9e166b-1444-4018-a9f6-62deeec58d12" alt=""><figcaption></figcaption></figure>

* O script foi executado em:`2024-05-21 03:16:32`&#x20;

***

**6- Qual é o caminho completo da ferramenta usada para realizar o ataque de Kerberoasting?**

* Usei o **PECmd**, de Eric Zimmerman, para extrair detalhes sobre o Prefetch correspondente:

```
PS C:\Users\johndoe\Desktop\Get-ZimmermanTools\net6> .\PECmd.exe -d "C:\Users\johndoe\Downloads\Triage\Workstation\2024-05-21T033012_triage_asset\C\Windows\prefetch" --csv C:\Users\johndoe\Downloads\ --csvf prefetch.csv
```

* Depois, carreguei o arquivo `prefetch.csv` no **Timeline Explorer** e procurei pelo executável associado ao Kerberoasting

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FwEnctryIESX7DqZvqPPw%2Fimage.png?alt=media&amp;token=87143f10-ce6e-4b5c-bca6-bbadab2969fc" alt=""><figcaption></figcaption></figure>

* A ferramenta utilizada para realizar o ataque de Kerberoasting estava localizada em:`\USERS\ALONZO.SPIRE\DOWNLOADS\RUBEUS.EXE`

***

**7- Quando a ferramenta foi executada para extrair as credenciais? (UTC)**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F8WzfpX62zj7X6BFBOJLf%2Fimage.png?alt=media&amp;token=63d8f773-0522-4e7f-afa1-fec833e403f0" alt=""><figcaption></figcaption></figure>
