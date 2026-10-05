> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/htb-sherlocks/logjammer.md).

# LogJammer

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FDgoGsHgYQYOroHxiJ8sI%2Fimage.png?alt=media&amp;token=aa04041c-b94a-4b8a-a26e-3c077dd4d693" alt="" width="375"><figcaption></figcaption></figure>

**Classificação**: Easy

**Categoria:** DFIR

**Cenário:**

> Você recebeu uma proposta para trabalhar como consultor júnior de DFIR (Digital Forwarding and Incidents) em uma grande consultoria. No entanto, eles lhe forneceram uma avaliação técnica para concluir. A consultoria Forela-Security gostaria de avaliar seu conhecimento em análise de logs de eventos do Windows. Acreditamos que o usuário Cyberjunkie acessou o computador e pode ter realizado ações maliciosas. Analise os logs de eventos fornecidos e apresente um relatório.

**Extrai todos os logs com a ferramenta EvtxECmd e os analisei com o Timeline Explorer**&#x20;

```
 .\EvtxECmd.exe -f "C:\Users\johndoe\Downloads\Event-Logs\System.evtx" --csv C:\Users\johndoe\ --csvf system.csv  
```

**1 - Quando o usuário cyberjunk conseguiu acessar seu computador pela primeira vez? (UTC)**

* Foram idenficados eventos de autenticação associados ao usuário Cyberjunkie, incluindo:
* Event ID 4624 - Logon bem-sucedido

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FNQ5qKOOyYDbF68Dcjzok%2Fimage.png?alt=media&amp;token=68cfc267-89b2-421f-ad76-9bb871d5a708" alt=""><figcaption></figcaption></figure>

**Resposta**: `2023/03/27 14:37:09`&#x20;

***

**2- O usuário alterou as configurações do firewall no sistema. Analise os registros de eventos do firewall para descobrir o nome da regra de firewall adicionada.**

* Suspeita-se que o usuário tenha alterado as configurações do firewall do sistema. Para confirmar essa ação e identificar a regra de firewall adicionada, foram analisados os logs do windows Firewall.
* Eventos relevantes:
* Event ID 2004 - Regra de firewall adicionada

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FtYoHPlyUtdyW8ODTYbdL%2Fimage.png?alt=media&amp;token=cb9b2750-c388-4db3-97fc-5db415f3ee63" alt=""><figcaption></figcaption></figure>

**Resposta**: `Metasploit C2 Bypass`&#x20;

***

**3- Qual é a direção da regra do firewall?**

Para identificar a direção da regra de firewall adicionada, foram analisados os eventos do log:

* Event ID 2004 - Regra de firewall adicionada
* Esse evento contém o campo Direction, que indica se a regra controla de entrada ou saída

Os valores possíveis são:

* **Inbound** - Tráfego de entrada
* **Outbound** - Tráfego de saída

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FBuwpg3qyTqBdsznXh2Y3%2Fimage.png?alt=media&amp;token=55972992-2ae8-4666-8367-f115fd6ef7cf" alt=""><figcaption></figcaption></figure>

**Resposta**: `Outbound`&#x20;

***

**4- O usuário alterou a política de auditoria do computador. Qual é a subcategoria dessa política alterada?**

Foi identificada uma alteração na política de auditoria do sistema por meio da análise dos **logs de segurança do Windows**.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FRUp3m3rerjYZbOJbbkvU%2Fimage.png?alt=media&amp;token=d013c6aa-16a4-4459-8eb6-90c87860b588" alt=""><figcaption></figcaption></figure>

* **Event ID 4719** – *System audit policy was changed*

Este evento registra explicitamente a **subcategoria da política de auditoria** que foi modificada.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fmchs2glgUv6Cw1kZdRcf%2Fimage.png?alt=media&amp;token=1f7061c6-6d79-4bc8-9f75-5a84a5e7653c" alt=""><figcaption></figcaption></figure>

**Resposta**: `Other System Events`&#x20;

***

**5 - O usuário "cyberjunkie" criou uma tarefa agendada. Qual o nome dessa tarefa?**

* Para identificar a tarefa agendada criada pelo usuário cyberjunkie, foi analisado o Security.evtx, com foco em eventos de criação de tarefas
* **Event ID 4698** – *A scheduled task was created*
* **SubjectUserName:** `cyberjunkie`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FNI9YDtOqAGQQGlqFSIcW%2Fimage.png?alt=media&amp;token=f932f3b0-6a59-4ae7-8b2b-ab5ee687041d" alt=""><figcaption></figcaption></figure>

**Resposta**: `HTB-AUTOMATION`&#x20;

***

**6- Qual é o caminho completo do arquivo que estava agendado para a tarefa?**

* **Event ID 4698** – *A scheduled task was created*
* **SubjectUserName:** `cyberjunkie`

Esse evento registra, além do nome da tarefa, o conteúdo do campo **Task Content**, que inclui:

* A **ação configurada**
* O **comando ou executável**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FqScGKxXRu8G9qE2FVNlz%2Fimage.png?alt=media&amp;token=d5c09d1d-63e3-487c-8f0f-45c9eeb2a41a" alt=""><figcaption></figcaption></figure>

**Resposta**: `C:\Users\CyberJunkie\Desktop\Automation-HTB.ps1`&#x20;

**7- Quais são os argumentos do comando?**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FpkTQ9GWt3JnZSRVVJPfT%2Fimage.png?alt=media&amp;token=a73fbeb6-cf3f-43ce-82ec-ea51c8e6c80c" alt=""><figcaption></figcaption></figure>

***

**8- O antivírus em execução no sistema identificou uma ameaça e realizou ações para neutralizá-la. Qual ferramenta foi identificada como malware pelo antivírus?**

Foram analisados os logs do **Microsoft Defender Antivirus**, especificamente:

* **Log:** `Microsoft-Windows-Windows Defender/Operational`
* **Event IDs comuns:**

  * **1116** – Malware detectado
  * **1117** – Malware removido

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FVzc24IHLkQWHOz1dfYXG%2Fimage.png?alt=media&amp;token=70763ef0-f1de-48fa-900f-b5a0d5820628" alt=""><figcaption></figcaption></figure>

**9- Qual é o caminho completo do malware que gerou o alerta?**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FVyawp1BO12K8yjxsOVGz%2Fimage.png?alt=media&amp;token=8ba67070-7d3b-4d01-9b24-b1f286b13e21" alt=""><figcaption></figcaption></figure>

***

**10- Que ação foi tomada pelo antivírus?**

A análise dos logs do **Microsoft Defender Antivirus** (`Microsoft-Windows-Windows Defender/Operational`) identificou que, após a detecção da ameaça, o antivírus executou automaticamente uma ação de mitigação.

* **Event ID 1116 / 1117**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FUcDnVAPHpvx41OGHNaSr%2Fimage.png?alt=media&amp;token=118fe8b8-b0eb-4a11-8216-dc48ad822a0b" alt=""><figcaption></figcaption></figure>

***

**11- O usuário utilizou o PowerShell para executar comandos. Qual comando foi executado pelo usuário?**

Durante a análise dos logs de eventos do Windows, foi identificado que o usuário **cyberjunkie** utilizou o **PowerShell** para executar comandos no sistema.

* **Log:** `Microsoft-Windows-PowerShell/Operational`
* **Event ID 4104** – *PowerShell Script Block Logging*

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FUtKPv3hgsxQaAfBjKTPO%2Fimage.png?alt=media&amp;token=92f60918-2049-4b9e-9fc1-5d172edb9316" alt=""><figcaption></figcaption></figure>
