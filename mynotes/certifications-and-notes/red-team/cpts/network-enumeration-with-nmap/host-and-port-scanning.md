> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/host-and-port-scanning.md).

# Host and Port Scanning

* **Escaneando as 10 principais portas TCP**

```sh
sudo nmap 10.129.2.28 --top-ports=10 
```

* **Nmap - Rastrear os Pacotes**

```sh
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

* **Conecte a digitalização na porta TCP 443 (-sT)**

```sh
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT 
```

* **Varredura de porta UDP (-sU)**

```sh
sudo nmap 10.129.2.28 -F -sU
```

`-F`  - Verifica as 100 principais portas

* **Version Scan (-sV)**

```sh
sudo nmap 10.129.2.28 -sV -Pn -n --disable-arp-ping --packet-trace -p 445 --reason  
```
