> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/pivoting-and-persistence/persistence/windows-service.md).

# Windows Service

**Objetivo**

Criar um serviço que execute o **implant do Sliver automaticamente no boot**, garantindo acesso persistente com **privilégios elevados (SYSTEM)**.

**Criar o serviço**

```
sc.exe create hacksmarter_service binPath= "C:\Users\t.ramsbey\persistence.exe" start= auto
```

&#x20;**Parâmetros:**

* `create` → cria serviço
* `hacksmarter_service` → nome do serviço
* `binPath=` → caminho do payload
* `start= auto` → inicia no boot

Atenção:

* Espaço obrigatório após `binPath=` e `start=`
* Requer execução como **admin**

**Reiniciar o sistema**

```
shutdown /r /t 0
```

**Características**

* Execução: **boot (startup)**
* Privilégio: **SYSTEM**
* Persistência: **muito alta**
* Execução: **background (stealth)**

**Vantagens**

* Alta confiabilidade
* Executa sem login de usuário
* Pode fornecer **elevação de privilégio**
* Mais stealth (sem interface)

**Desvantagens**

* Requer privilégios administrativos
* Pode ser monitorado por EDR/Defender
