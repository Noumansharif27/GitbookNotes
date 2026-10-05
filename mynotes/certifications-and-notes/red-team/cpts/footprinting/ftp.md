> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/ftp.md).

# FTP

* **Scripts FTP Nmap**

```sh
sudo nmap --script-updatedb
```

* Encontrar todos os scripts relacionados ao ftp

```sh
find / -type f -name ftp* 2>/dev/null | grep scripts
```

* Enumerar a porta 21 com o **nmap**

```sh
sudo nmap -sV -p21 -sC -A <FQDN/IP>
```

* **Rastreamento de script Nmap**

```sh
sudo nmap -sV -p21 -sC -A <FQDN/IP> --script-trace
```

* Interaja com o serviço FTP no destino.

```sh
ftp <FQDN/IP>
```

* Interaja com o serviço FTP no destino.

```sh
nc -nv <FQDN/IP> 21
```

* Interaja com o serviço FTP no destino.

```sh
telnet <FQDN/IP> 21
```

* Interaja com o serviço FTP no destino usando conexão criptografada.

```sh
openssl s_client -connect <FQDN/IP>:21 -starttls ftp
```

* Baixe todos os arquivos disponíveis no servidor FTP de destino.

```sh
wget -m --no-passive ftp://anonymous:anonymous@<target>
```
