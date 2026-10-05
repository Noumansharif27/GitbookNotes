> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/port-forwarding-with-windows-netsh.md).

# Port Forwarding with Windows Netsh

[Netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) é uma ferramenta de linha de comando do Windows que pode ajudar com a configuração de rede de um sistema Windows específico. Aqui estão apenas algumas das tarefas relacionadas à rede que podemos usar `Netsh`para:

* `Finding routes`

* `Viewing the firewall configuration`

* `Adding proxies`

* `Creating port forwarding rules`

* **Usando Netsh.exe para encaminhamento de porta**

```sh
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25
```

* **Verificando o encaminhamento de porta**

```sh
netsh.exe interface portproxy show v4tov4
```
