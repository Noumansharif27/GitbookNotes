> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/listener-and-implant-creation/personalizacao-de-implantes.md).

# Personalização de Implantes

A escolha do implant correto é essencial e depende principalmente do **sistema operacional alvo** e do **tipo de payload**.

**Tipos de Implantes por Sistema Operacional**

&#x20;**Windows**

* `.exe` → Executável padrão
* `.dll` → Biblioteca usada por outros programas (execução indireta)

**Linux**

* **ELF (Executable and Linkable Format)**
* Formato padrão para:
  * Executáveis
  * Bibliotecas
  * Core dumps

**macOS**

* **Mach-O (Mach Object)**
* Formato nativo da Apple (macOS/iOS)
* Equivalente ao `.exe` no Windows

**Tipos de Payload**

**Carga útil em estágios (Staged)**

* **Tamanho:** Pequeno
* **Funcionamento:**
  * Baixa o restante do payload do C2
* **Vantagens:**
  * Mais discreto (ideal para exploits com limite de tamanho)
* **Desvantagens:**
  * Depende de segunda conexão
  * Maior risco de falha/detecção

**Carga útil sem estágios (Stageless)**

* **Tamanho:** Maior
* **Funcionamento:**
  * Já contém tudo necessário
* **Vantagens:**
  * Mais confiável
  * Não depende de download adicional
* **Desvantagens:**
  * Pode ser mais fácil de detectar pelo tamanho
