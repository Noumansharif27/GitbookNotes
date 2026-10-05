> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/imap-pop3/question.md).

# Question

1. Descubra o nome exato da organização no serviço IMAP/POP3 e envie-o como resposta.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FuYYFCVKhusl7zkiILzC0%2Fimage.png?alt=media&amp;token=d78909c7-3064-4fb1-83be-e421b8cc7f5b" alt=""><figcaption></figcaption></figure>

2. Qual é o FQDN ao qual os servidores IMAP e POP3 são atribuídos?

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F497pJQg4mI2CwicVNySp%2Fimage.png?alt=media&amp;token=e7eee385-a07e-4e32-97e0-b9bfbbd75330" alt=""><figcaption></figcaption></figure>

3. Enumere o serviço IMAP e envie o sinalizador como resposta. (Formato: HTB{...})

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FRwZYdQdji8CMh8n6NHW5%2Fimage.png?alt=media&amp;token=4302ab7b-405b-483a-8945-ad8d1bff8306" alt=""><figcaption></figcaption></figure>

4. Qual é a versão personalizada do servidor POP3?

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FH3owYLCDO1nShPfxpL2i%2Fimage.png?alt=media&amp;token=4ffc8b57-dd19-48b7-94b0-ff8df1802de6" alt=""><figcaption></figcaption></figure>

5. Qual é o endereço de e-mail do administrador?

* Logar na maquina usando o openssl

```sh
openssl s_client -connect 10.129.146.129:imaps
```

* Usar as credenciais robin:robin para conseguir acesso ao servidor

```sh
A1 LOGIN "robin" "robin"
```

* Listar todas as caixas de correio

```sh
A1 LIST "" *

* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
```

* Identificamos para varias caixas de correios, a pasta de correio dev.department.int, possui subpastas diferente das outras caixas, vamos seleciona-lo

```sh
A3 SELECT DEV.DEPARTMENT.INT

* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 1 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636414279] UIDs valid
* OK [UIDNEXT 2] Predicted next UID
```

* Obter informações sobre a primeira mensagem

```sh
A5 FETCH 1 (FLAGS BODY.PEEK[])

* 1 FETCH (FLAGS (\Seen) BODY[] {167}
Subject: Flag
To: Robin <robin@inlanefreight.htb>
From: CTO <devadmin@inlanefreight.htb>
Date: Wed, 03 Nov 2021 16:13:27 +0200

HTB{983uzn8jmfgpd8jmof8c34n7zio}
```
