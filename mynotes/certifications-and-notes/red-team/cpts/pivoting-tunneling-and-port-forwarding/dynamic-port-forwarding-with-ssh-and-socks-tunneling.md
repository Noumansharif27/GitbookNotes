> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/dynamic-port-forwarding-with-ssh-and-socks-tunneling.md).

# Dynamic Port Forwarding with SSH and SOCKS Tunneling

`Port forwarding`é uma técnica que nos permite redirecionar uma solicitação de comunicação de uma porta para outra.&#x20;

* **Scanning the Pivot Target**

```sh
nmap -sT -p22,3306 <IPaddressofTarget>
```

* **Executando o encaminhamento de porta local**

```sh
ssh -L 1234:localhost:3306 ubuntu@<IPaddressofTarget>
```

* O paramêtro  `-L` diz ao cliente SSH para solicitar ao servidor SSH que encaminhe todos os dados que enviamos pela porta `1234`para `localhost:3306`o servidor Ubuntu.
* **Confirmando Port Forward com Netstat**

```sh
netstat -antp | grep 1234
```

* **Confirmando Port Forward com Nmap**

```sh
nmap -v -sV -p1234 localhost
```

* **Encaminhando várias portas**

```sh
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@<IPaddressofTarget>
```

***

### **SSH Dynamic**

* **Habilitando o encaminhamento dinâmico de portas com SSH**

```sh
ssh -D 9050 ubuntu@<IPaddressofTarget>
```

* Para informar ao proxychains que devemos usar a porta 9050, precisamos modificar o arquivo de configuração do proxychains localizado em `/etc/proxychains.conf`. Podemos adicionar `socks4 127.0.0.1 9050`à última linha se ainda não estiver lá.
* **Verificando /etc/proxychains.conf**

```sh
tail -4 /etc/proxychains.conf
```

* **Usando Nmap com Proxychains**

```sh
proxychains nmap -v -sn 172.16.5.1-200
```

* **Enumerando o alvo do Windows por meio de Proxychains**

```
proxychains nmap -v -Pn -sT 172.16.5.19
```

***

### Usando Metasploit com Proxychains

* **Iniciar msfconsole**

```sh
proxychains msfconsole
```

* **Usando o módulo rdp\_scanner**

```sh
auxiliary/scanner/rdp/rdp_scanner
```

* **Usando xfreerdp com Proxychains**

```sh
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
