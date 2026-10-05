> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/cheat-sheet-nmap/scanning-options.md).

# Scanning Options

| **Opção Nmap**       | **Descrição**                                                                                             |
| -------------------- | --------------------------------------------------------------------------------------------------------- |
| `10.10.10.0/24`      | Alcance da rede alvo.                                                                                     |
| `-sn`                | Desabilita a varredura de portas.                                                                         |
| `-Pn`                | Desabilita solicitações de eco ICMP                                                                       |
| `-n`                 | Desabilita a resolução de DNS.                                                                            |
| `-PE`                | Executa a varredura de ping usando solicitações de eco ICMP no alvo.                                      |
| `--packet-trace`     | Mostra todos os pacotes enviados e recebidos.                                                             |
| `--reason`           | Exibe o motivo de um resultado específico.                                                                |
| `--disable-arp-ping` | Desabilita solicitações de ping ARP.                                                                      |
| `--top-ports=<num>`  | Verifica as portas principais especificadas que foram definidas como mais frequentes.                     |
| `-p-`                | Escaneie todas as portas.                                                                                 |
| `-p22-110`           | Verifique todas as portas entre 22 e 110.                                                                 |
| `-p22,25`            | Verifica apenas as portas especificadas 22 e 25.                                                          |
| `-F`                 | Verifica as 100 principais portas.                                                                        |
| `-sS`                | Executa um TCP SYN-Scan.                                                                                  |
| `-sA`                | Executa um TCP ACK-Scan.                                                                                  |
| `-sU`                | Executa uma varredura UDP.                                                                                |
| `-sV`                | Verifica os serviços descobertos em busca de suas versões.                                                |
| `-sC`                | Execute uma verificação de script com scripts categorizados como "padrão".                                |
| `--script <script>`  | Executa uma verificação de script usando os scripts especificados.                                        |
| `-O`                 | Executa uma verificação de detecção de sistema operacional para determinar o sistema operacional do alvo. |
| `-A`                 | Executa detecção de SO, detecção de serviço e varreduras de traceroute.                                   |
| `-D RND:5`           | Define o número de chamarizes aleatórios que serão usados ​​para escanear o alvo.                         |
| `-e`                 | Especifica a interface de rede usada para a verificação.                                                  |
| `-S 10.10.10.200`    | Especifica o endereço IP de origem para a verificação.                                                    |
| `-g`                 | Especifica a porta de origem para a digitalização.                                                        |
| `--dns-server <ns>`  | A resolução de DNS é realizada usando um servidor de nomes especificado.                                  |
