> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/bypass-security-measures/firewall-and-ids-ips-evasion.md).

# Firewall and IDS/IPS Evasion

* **SYN-Varredura**

```sh
sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace
```

* **Scan by Using Decoys**

Nmap gera vários endereços IP aleatórios inseridos no cabeçalho IP para disfarçar a origem do pacote enviado.

```sh
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
```

* **Digitalizar usando IP de origem diferente**

```sh
sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```

* **Proxy DNS**

Como outro exemplo, podemos usar `TCP port 53`como uma porta de origem ( `--source-port`) para nossas varreduras. Se o administrador usar o firewall para controlar esta porta e não filtrar IDS/IPS corretamente, nossos pacotes TCP serão confiáveis ​​e passarão por ela.

{% tabs %}
{% tab title="Exemplo 1 -  SYN-Scan de uma porta filtrada" %}

```sh
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 22:50 CEST
SENT (0.0417s) TCP 10.10.14.2:33436 > 10.129.2.28:50000 S ttl=41 id=21939 iplen=44  seq=736533153 win=1024 <mss 1460>
SENT (1.0481s) TCP 10.10.14.2:33437 > 10.129.2.28:50000 S ttl=46 id=6446 iplen=44  seq=736598688 win=1024 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up.

PORT      STATE    SERVICE
50000/tcp filtered ibm-db2 <----

Nmap done: 1 IP address (1 host up) scanned in 2.06 seconds
```

{% endtab %}

{% tab title="Exemplo 2 - SYN-Scan da porta DNS " %}

```sh
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53

SENT (0.0482s) TCP 10.10.14.2:53 > 10.129.2.28:50000 S ttl=58 id=27470 iplen=44  seq=4003923435 win=1024 <mss 1460>
RCVD (0.0608s) TCP 10.129.2.28:50000 > 10.10.14.2:53 SA ttl=64 id=0 iplen=44  seq=540635485 win=64240 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up (0.013s latency).

PORT      STATE SERVICE
50000/tcp open  ibm-db2 <----
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

{% endtab %}
{% endtabs %}
