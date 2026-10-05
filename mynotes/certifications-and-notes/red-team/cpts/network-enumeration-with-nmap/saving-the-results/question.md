> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/saving-the-results/question.md).

# Question

1. Execute uma varredura completa da porta TCP no seu alvo e crie um relatório HTML. Envie o número da porta mais alta como resposta.

* Executar uma varredura e salvar no formato xml

```
sudo nmap --open -oX target --disable-arp-ping 10.129.84.55
 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-20 17:59 CDT
Nmap scan report for 10.129.84.55
Host is up (0.069s latency).
Not shown: 993 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
110/tcp   open  pop3
139/tcp   open  netbios-ssn
143/tcp   open  imap
445/tcp   open  microsoft-ds
31337/tcp open  Elite
```

* Instalar o `xsltproc` no linux&#x20;

```
sudo apt-get install xsltproc
```

* Converter os resultados

```
xsltproc target.xml -o target.html
```

* Acessar o navegador e abrir o novo formato gerado da varredura com o `nmap`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FwgRPapG6gV7MsDNmjz1X%2Fimage.png?alt=media&amp;token=001ed49f-8bc2-481e-8ca4-2866b818d245" alt=""><figcaption></figcaption></figure>
