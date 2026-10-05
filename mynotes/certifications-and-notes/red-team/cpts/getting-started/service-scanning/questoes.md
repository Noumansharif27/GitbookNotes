> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/service-scanning/questoes.md).

# Questões

1. &#x20;Execute uma varredura Nmap do alvo. O que o Nmap exibe como a versão do serviço em execução na porta 8080?

```
$ nmap -sC -sV -p8080 10.129.96.71 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-14 13:47 CDT
Nmap scan report for 10.129.96.71
Host is up (0.068s latency).

PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.81 seconds
```

2. Execute uma varredura Nmap do alvo e identifique a porta não padrão na qual o serviço telnet está sendo executado.

```
$ nmap -sV 10.129.96.71 

...SNIP...
2323/tcp open  telnet      Linux telnetd
```

3. Liste os compartilhamentos SMB disponíveis no host de destino. Conecte-se ao compartilhamento disponível como o usuário bob. Uma vez conectado, acesse a pasta chamada 'flag' e envie o conteúdo do arquivo flag.txt.

```
$ smbclient -U bob%Welcome1 \\\\10.129.96.71\\users
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Feb 25 17:06:52 2021
  ..                                  D        0  Thu Feb 25 14:05:31 2021
  flag                                D        0  Thu Feb 25 17:09:26 2021
  bob                                 D        0  Thu Feb 25 15:42:23 2021

		4062912 blocks of size 1024. 1276104 blocks available
smb: \> 
```
