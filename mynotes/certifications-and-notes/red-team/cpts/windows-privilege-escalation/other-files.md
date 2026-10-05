> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/other-files.md).

# Other Files

* **Pesquisar conteúdo de arquivo por uma determinada string - Exemplo 1**

```ps
cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt
```

* **Pesquisar conteúdo de arquivo por uma determinada string - Exemplo 2**

```ps
findstr /si password *.xml *.ini *.txt *.config
```

* **Pesquisar conteúdo de arquivo por uma determinada string - Exemplo 3**

```ps
findstr /spin "password" *.*
```

* **Pesquisar conteúdo de arquivos com o PowerShell**

```ps
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password
```

* **Pesquisa por extensões de arquivo - Exemplo 1**

```ps
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*
```

* **Pesquisar extensões de arquivo usando o PowerShell**

```ps
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```

* **Visualizando dados de notas adesivas usando o PowerShell**

```ps
Set-ExecutionPolicy Bypass -Scope Process
```
