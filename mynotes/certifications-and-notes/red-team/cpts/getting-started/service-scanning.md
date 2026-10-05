> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/service-scanning.md).

# Service Scanning

* Execute o nmap em um IP

```sh
nmap 10.129.42.253
```

* Execute uma varredura de script nmap em um IP

```sh
nmap -sV -sC -p- 10.129.42.253
```

* Listar vários scripts nmap disponíveis

```sh
locate scripts/citrix
```

* Execute um script nmap em um IP

```sh
nmap --script smb-os-discovery.nse -p445 10.10.10.40
```

* Pegue o banner de uma porta aberta

```sh
netcat 10.10.10.10 22
```

* Listar ações de SMB's

```sh
smbclient -N -L \\\\10.129.42.253
```

* Conectar a um compartilhamento SMB

```sh
smbclient \\\\10.129.42.253\\users
```

* Escanear SNMP em um IP

```sh
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
```

* String secreta SNMP de força bruta

```sh
onesixtyone -c dict.txt 10.129.42.254
```
