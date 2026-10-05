> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/performance.md).

# Performance

O desempenho da varredura desempenha um papel significativo quando precisamos varrer uma rede extensa ou estamos lidando com baixa largura de banda de rede. Podemos usar várias opções para dizer `Nmap`quão rápido ( `-T <0-5>`), com qual frequência ( `--min-parallelism <number>`), quais timeouts ( `--max-rtt-timeout <time>`) os pacotes de teste devem ter, quantos pacotes devem ser enviados simultaneamente ( `--min-rate <number>`), e com o número de tentativas ( `--max-retries <number>`) para as portas varridas os alvos devem ser varridos.

* **Verificação padrão**

```sh
sudo nmap 10.129.2.0/24 -F
```

* **RTT otimizado**

```sh
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

| `--initial-rtt-timeout 50ms` | Define o valor de tempo especificado como tempo limite de RTT inicial. |
| ---------------------------- | ---------------------------------------------------------------------- |
| `--max-rtt-timeout 100ms`    | Define o valor de tempo especificado como tempo limite máximo de RTT.  |

* **Tempo**

Como essas configurações nem sempre podem ser otimizadas manualmente, como em um teste de penetração de caixa preta, `Nmap`oferece seis modelos de tempo diferentes ( `-T <0-5>`) para usarmos.

* `-T 0`/`-T paranoid`

* `-T 1`/`-T sneaky`

* `-T 2`/`-T polite`

* `-T 3`/`-T normal`

* `-T 4`/`-T aggressive`

* `-T 5`/`-T insane`

* **Verificação padrão**

```sh
Akira20@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default 

<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 32.44 seconds
```

* **Varredura insana**

```sh
sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5

<SNIP>
Nmap done: 256 IP addresses (10 hosts up) scanned in 18.07 seconds
```
