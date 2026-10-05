> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/ssh-pivoting-with-sshuttle.md).

# SSH Pivoting with Sshuttle

[Sshuttle](https://github.com/sshuttle/sshuttle) é outra ferramenta escrita em Python que elimina a necessidade de configurar proxychains. No entanto, essa ferramenta só funciona para pivotar sobre SSH e não fornece outras opções para pivotar sobre servidores proxy TOR ou HTTPS.Um uso interessante do sshuttle é que não precisamos usar proxychains para conectar aos hosts remotos.&#x20;

* **Instalando sshuttle**

```sh
sudo apt-get install sshuttle
```

* **Running sshuttle**

```sh
sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v
```

* **Roteamento de tráfego através de rotas iptables**

```sh
nmap -v -sV -p3389 172.16.5.19 -A -Pn
```
