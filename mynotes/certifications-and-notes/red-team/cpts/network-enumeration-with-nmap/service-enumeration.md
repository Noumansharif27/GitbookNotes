> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/service-enumeration.md).

# Service Enumeration

* **Detecção de versão de serviço**

```sh
sudo nmap 10.129.2.28 -p- -sV
```

* Definir por quantos períodos de tempo o status deve ser exibido

```sh
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```

`--stats-every=5s`  - Mostra o progresso da verificação a cada 5 segundos.

* Também podemos aumentar o `verbosity level`( `-v`/ `-vv`), que nos mostrará as portas abertas diretamente quando `Nmap`as detectar.

```sh
sudo nmap 10.129.2.28 -p- -sV -v 
```

* **Tcpdump**

```sh
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

* **Nc**

```sh
nc -nv 10.129.2.28 25
```
