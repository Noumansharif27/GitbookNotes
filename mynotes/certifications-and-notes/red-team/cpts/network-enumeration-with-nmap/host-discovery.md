> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/host-discovery.md).

# Host Discovery

* **Alcance da rede de digitalização**

```sh
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

* **Lista de IP de digitalização**

```sh
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

* **Escanear vários IPs**

```sh
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```

* Se esses endereços IP estiverem próximos um do outro, também podemos definir o intervalo no respectivo octeto.

```sh
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```

* **Digitalizar IP Único**

{% tabs %}
{% tab title="Versão 1" %}

```sh
sudo nmap 10.129.2.18 -sn -oA host 
```

{% endtab %}

{% tab title="Versão 2" %}

```sh
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace 
```

* `PE`  - Executa a varredura de ping usando 'solicitações de echo ICMP' no alvo.

* `--packet-trace`  - Mostra todos os pacotes enviados e recebidos
  {% endtab %}
  {% endtabs %}

* Outra maneira de determinar por que o Nmap marcou nosso alvo como "vivo" é com a opção  `--reason`.

```sh
sudo nmap 10.129.2.18 -sn -oA host -PE --reason 
```

* Desabilitar arp-ping&#x20;

```sh
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 
```
