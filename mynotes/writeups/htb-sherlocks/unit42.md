> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/htb-sherlocks/unit42.md).

# Unit42

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FaIDOJXuqAUQOjcov08C9%2Fimage.png?alt=media&amp;token=0b7a3cb6-bbf8-4691-93ec-37ce01497b17" alt="" width="375"><figcaption></figcaption></figure>

**Classificação**:  Very Easy

**Categoria:** DFIR

**Cenário:**<br>

> Neste laboratório Sherlock, você se familiarizará com os logs do Sysmon e vários IDs de evento úteis para identificar e analisar atividades maliciosas em um sistema Windows. A Unit42 de Palo Alto conduziu recentemente uma pesquisa sobre uma campanha do UltraVNC, na qual os atacantes utilizaram uma versão com backdoor do UltraVNC para manter o acesso aos sistemas. Este laboratório é inspirado nessa campanha e guia os participantes pela fase inicial de acesso.

#### Questões

**1- Quantos registros de eventos existem com o ID de evento 11?**

* Analisar o log sysmon com a ferramenta **EvtxECmd.exe**, de Eric Zimmerman e gerar os resultados em formato CSV

```
PS C:\Users\johndoe\Desktop\Get-ZimmermanTools\net6\EvtxeCmd> .\EvtxECmd.exe -f C:\Users\johndoe\Downloads\Microsoft-Windows-Sysmon-Operational.evtx --csv C:\Users\johndoe\ --csvf sysmon1.csv
```

* A saída exibirá um resumo rápido do número de eventos para cada ID de evento so Sysmon.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FGj7zXTOrQEwqKGJLRgML%2Fimage.png?alt=media&amp;token=2fda75aa-0861-4f0b-a772-6d6d28818cd4" alt=""><figcaption></figcaption></figure>

**Resposta**: `56`

***

**2- Sempre que um processo é criado na memória, um evento com ID 1 é registrado com detalhes como linha de comando, hashes, caminho do processo, caminho do processo pai, etc. Essas informações são muito úteis para um analista, pois permitem visualizar todos os programas executados em um sistema, o que significa que podemos identificar quaisquer processos maliciosos em execução. Qual é o processo malicioso que infectou o sistema da vítima?**

* Para identificar qual processo malicioso infectou o sistema, analisei os eventos de criação de processo (Sysmon Event ID 1).
* Após extrair o Sysmon via EvtxECmd e carregar os dados no Timeline Explorer, filtrei por EventID = 1 e investiguei processos executados a partir de diretórios suspeitos.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FgD08VlB8tNn15v7PF8UT%2Fimage.png?alt=media&amp;token=8387c84f-1514-47e8-bd86-67264fe3cd08" alt=""><figcaption></figcaption></figure>

**Resposta:** `C:\Users\CyberJunkie\Downloads\Preventivo24.02.14.exe.exe`

***

3- **Qual serviço de armazenamento em nuvem foi usado para distribuir o malware?**

* Para identificar o serviço de nuvem utilizado para distribuir o malware, analisei os eventos Sysmon responsáveis por criação de processos, conexões de rede e consultas DNS (Event ID 1, 3 e 22).

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FdqV0ZRGENcrbt1QDCS2f%2Fimage.png?alt=media&amp;token=9aad2819-0bbf-4ab5-875c-0990d95042c1" alt=""><figcaption></figcaption></figure>

* os registros de data e hora demonstram que o arquivo malicioso 'Preventivo24.02.14.exe.exe' foi baixado segundos após o usuário acessar dropbox.com

**Resposta:** `Dropbox`

***

**4- O arquivo malicioso inicial alterou a data e hora (uma técnica de evasão de defesa, na qual a data de criação do arquivo é alterada para parecer antiga) de muitos arquivos que criou no disco. Qual foi a data e hora alterada para um arquivo PDF?**

* O ID de evento 2 do Sysmon (Um processo alterou a hora de criação de um arquivo) pode ser usado para identificar alterações de data e hora. Vou criar um filtro global para 'PDF' e, em seguida, expandir o ID de evento 2:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsDRNBR3OPI7QHCnc1MDS%2Fimage.png?alt=media&amp;token=bb58d2de-1fd6-4f2a-8287-dfba2e44a18a" alt=""><figcaption></figcaption></figure>

**Resposta:** `2024-01-14 08:10:06`<br>

***

**5- O arquivo malicioso criou alguns arquivos no disco. Onde o arquivo “once.cmd” foi criado no disco? Por favor, responda com o caminho completo e o nome do arquivo.**

* Ao analisar os eventos Sysmon de criação de arquivo (Event ID 11) no Timeline Explorer, filtrei o log para localizar o arquivo *once.cmd* criado pelo malware.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FpxAOxeyFXBj5Vd9FJu2Y%2Fimage.png?alt=media&amp;token=8577a994-333b-4b6b-8cda-268358be9f92" alt=""><figcaption></figcaption></figure>

* O campo **TargetFilename** indicou o caminho completo onde o arquivo foi escrito no disco e posteriormente movido para o diretório ***C:\Games***

**Resposta:** `C:\Users\CyberJunkie\AppData\Roaming\Photo and Fax Vn\Photo and vn 1.1.2\install\F97891C\WindowsVolume\Games\once.cmd`

***

**6- O arquivo malicioso tentou acessar um domínio fictício, provavelmente para verificar o status da conexão com a internet. A qual domínio ele tentou se conectar?**

* Para identificar o domínio que o malware tentou acessar, analisei os eventos Sysmon de rede (Event ID 3) e consultas DNS (Event ID 22) no Timeline Explorer.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fwz6UJeq4THfAZ6JjC4VY%2Fimage.png?alt=media&amp;token=de050930-1133-49d4-b374-1a9f290c83fa" alt=""><figcaption></figcaption></figure>

* Ao filtrar esses eventos e procurar por domínios externos, encontrei um domínio fictício acessado pelo executável malicioso

**Resposta:** `www.example.com`

***

**7- Qual endereço IP o processo malicioso tentou contatar?**

* Ao analisar os eventos Sysmon de conexão de rede (Event ID 3) no Timeline Explorer, filtrei os eventos gerados pelo processo malicioso identificado anteriormente.\
  Na coluna **DestinationIp**, encontrei o endereço IP que o malware tentou contatar

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FeT4JbNxBTCS5Cu15ulNp%2Fimage.png?alt=media&amp;token=52b40081-1d06-4087-8d61-9dc5cadb4130" alt=""><figcaption></figcaption></figure>

**Resposta:** `93.184.216.34`

***

**8- O processo malicioso se encerrou após infectar o PC com uma variante do UltraVNC com backdoor. Quando o processo se encerrou?**

* Para determinar quando o processo malicioso foi encerrado, filtrei todos os eventos do Sysmon associados ao seu ProcessGuid no Timeline Explorer.
* A partir disso, identifiquei o evento **Process Terminated (Event ID 5)**, que registra a finalização do processo.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FeWhA074iYmWMi1DIQeHv%2Fimage.png?alt=media&amp;token=38a10acf-d1ea-4db5-b6bf-d2225eb1324d" alt=""><figcaption></figcaption></figure>

**Resposta:** `2024-02-14 03:41:58`
