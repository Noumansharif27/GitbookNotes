> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/nmap-scripting-engine.md).

# Nmap Scripting Engine

Nmap Scripting Engine ( `NSE`) é outro recurso útil do `Nmap`. Ele nos fornece a possibilidade de criar scripts em Lua para interação com certos serviços.&#x20;

* **Scripts Padrão**

```sh
sudo nmap <target> -sC
```

* **Categoria de scripts específicos**

```sh
sudo nmap <target> --script <category>
```

* **Scripts definidos**

```sh
sudo nmap <target> --script <script-name>,<script-name>,...
```

* **Nmap - Especificando Scripts**

```sh
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```

* **Nmap - Varredura Agressiva**

```sh
sudo nmap 10.129.2.28 -p 80 -A
```

`-A`  - Executa detecção de serviço, detecção de SO, traceroute e usa scripts padrões para escanear o alvo.

* **Nmap - Categoria Vuln**

```sh
sudo nmap 10.129.2.28 -p 80 -sV --script vuln 
```

`--script vuln` - Usa todos os scripts relacionados da categoria especificada.
