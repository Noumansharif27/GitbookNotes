> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/icmp-tunneling-with-socks.md).

# ICMP Tunneling with SOCKS

O tunelamento ICMP encapsula seu tráfego dentro `ICMP packets`de `echo requests`e `responses`. O tunelamento ICMP só funcionaria quando as respostas de ping fossem permitidas dentro de uma rede com firewall.&#x20;

* **Clonagem Ptunnel-ng**

```sh
git clone https://github.com/utoni/ptunnel-ng.git
```

* Depois que o repositório ptunnel-ng for clonado em nosso host de ataque, podemos executar o `autogen.sh`script localizado na raiz do diretório ptunnel-ng.

```sh
sudo ./autogen.sh 
```

* **Transferindo Ptunnel-ng para o Pivot Host**

```sh
scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```

* **Iniciando o servidor ptunnel-ng no host de destino**

```sh
sudo ./ptunnel-ng -r10.129.202.64 -R22
```

* O endereço IP a seguir `-r`deve ser o IP em que queremos que o ptunnel-ng aceite conexões.
* **Conectando ao servidor ptunnel-ng do host de ataque**

```sh
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```

* **Tunelamento de uma conexão SSH através de um túnel ICMP**

```sh
ssh -p2222 -lubuntu 127.0.0.1
```

* **Habilitando o encaminhamento dinâmico de portas via SSH**

```sh
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```

* **Encadeamento de proxy através do túnel ICMP**

```sh
proxychains nmap -sV -sT 172.16.5.19 -p3389
```
