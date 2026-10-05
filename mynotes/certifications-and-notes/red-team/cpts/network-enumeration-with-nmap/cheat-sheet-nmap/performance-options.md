> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/cheat-sheet-nmap/performance-options.md).

# Performance Options

| **Opção Nmap**               | **Descrição**                                                          |
| ---------------------------- | ---------------------------------------------------------------------- |
| `--max-retries <num>`        | Define o número de tentativas para varreduras de portas específicas.   |
| `--stats-every=5s`           | Exibe o status da verificação a cada 5 segundos.                       |
| `-v/-vv`                     | Exibe a saída detalhada durante a verificação.                         |
| `--initial-rtt-timeout 50ms` | Define o valor de tempo especificado como tempo limite de RTT inicial. |
| `--max-rtt-timeout 100ms`    | Define o valor de tempo especificado como tempo limite máximo de RTT.  |
| `--min-rate 300`             | Define o número de pacotes que serão enviados simultaneamente.         |
| `-T <0-5>`                   | Especifica o modelo de tempo específico.                               |
