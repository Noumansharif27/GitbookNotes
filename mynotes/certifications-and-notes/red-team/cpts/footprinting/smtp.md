> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/smtp.md).

# SMTP

O `Simple Mail Transfer Protocol`( `SMTP`) é um protocolo para enviar e-mails em uma rede IP. Ele pode ser usado entre um cliente de e-mail e um servidor de e-mail de saída ou entre dois servidores SMTP. O SMTP é frequentemente combinado com os protocolos IMAP ou POP3, que podem buscar e-mails e enviar e-mails.&#x20;

* **Nmap**

```sh
sudo nmap 10.129.14.128 -sC -sV -p25
```

* **Nmap - Relé Aberto**

```sh
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```

* **Telnet**

```sh
telnet 10.129.14.128 25
```

* O comando `VRFY`pode ser usado para enumerar usuários existentes no sistema.

```shell-session
VRFY root
```
