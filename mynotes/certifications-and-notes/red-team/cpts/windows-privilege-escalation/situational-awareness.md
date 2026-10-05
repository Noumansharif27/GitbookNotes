> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/situational-awareness.md).

# Situational Awareness

* **Interface(s), endereço(s) IP, informações de DNS**

```powershell
ipconfig /all
```

* **Tabela ARP**

```powershell
arp -a
```

* **Tabela de roteamento**

```
route print
```

* **Verificar o status do Windows Defender**

```powershell
Get-MpComputerStatus
```

* **Listar regras do AppLocker**

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

* **Política de teste do AppLocker**

```
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```
