> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/communication-with-processes.md).

# Communication with Processes

* **Exibir conexões de rede ativas**

```
netstat -ano
```

**Mais sobre Pipes Nomeados**

Pipes são usados ​​para comunicação entre dois aplicativos ou processos que utilizam memória compartilhada.

* **Lista de tubulações com nomes específicos usando o Pipelist**

```powershell
pipelist.exe /accepteula
```

* **Listando pipes nomeados com o PowerShell**

```powershell
gci \\.\pipe\
```

* Após obter uma lista de pipes nomeados, podemos usar [o Accesschk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) para enumerar as permissões atribuídas a um pipe nomeado específico

```powershell
accesschk.exe /accepteula \\.\Pipe\lsass -v
```

**Exemplo de ataque de pipes nomeados**

Vamos analisar um exemplo de como aproveitar um pipe nomeado exposto para escalar privilégios

* **Verificando as permissões do Named Pipe do WindscribeService**

```powershell
accesschk.exe -accepteula -w \pipe\WindscribeService -v
```
