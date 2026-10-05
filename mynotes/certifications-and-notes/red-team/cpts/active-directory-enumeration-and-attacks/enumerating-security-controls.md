> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/enumerating-security-controls.md).

# Enumerating Security Controls

* **Verificando o status do Defender com Get-MpComputerStatus**

```powershell
Get-MpComputerStatus
```

* **Usando o cmdlet Get-AppLockerPolicy**

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

* **Usando Find-LAPSDelegatedGroups**

```powershell
Find-LAPSDelegatedGroups
```

* **Usando Get-LAPSComputers**

```powershell
Get-LAPSComputers
```
