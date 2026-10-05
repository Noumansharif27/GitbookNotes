> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/intro-to-the-cli.md).

# Intro to the CLI

* **Cisco IOS**

É um sistema operacional usado em dispositivos Cisco, como Windows em um PC ou macOS em um Imac. Devemos ter em mente que o IOS da Cisco não está relacionado ao iOS da Apple para IPhones.

* **CLI (Comman-Line Interface)**

É a interface que usamos para configurar dispositivos Cisco, como roteadores, switches e firewalls, usamos um cabo console para nos conectarmos a porta do console desses dispositivos.

Normalmente em um dispositivo real, usamos um software chamado `Putty` para serial, como alternativa também usamos conexão remota (`ssh`)

* **User Exec Mode**

`Router>` : Indica o modo user, esse estado possui poucos privilégios

* **Privileged EXEC Mode**

`enable` **:**  Move um usuário do modo user exec para o modo privilegiado

`?`  : Usamos o ponto de interrogação para visualizar os comandos disponíveis

* **Global Configuration Mode**

`configure terminal` : Utilizado para entrar no modo de configuração&#x20;

* **Enable password command**

`enable password CCNA` : Hablitar uma senha, protege o modo exec privilegiado com uma senha

`exit` : Retornar ao modo anterior&#x20;

* **running-config & startup-config**&#x20;

`show running-config` : Exibe a configuração atual do roteador em execução.

`show startup-config` : Exibe a configuração que será aplicada ao roteador na próxima inicialização. Essa configuração está armazenada na memória NVRAM.

* **Saving the configuration**

`write memory` : É utilizado para salvar as configurações atuais do roteador, que estão na memória em execução (running-config), na memória NVRAM (startup-config).

`copy running-config startup-config` : Copiar o arquivo running-config para o arquivo startup-config

* **Service passoword-encryption command**

`conf t` : Entrar no modo de configuração

`service password-encryption` : habilita a criptografia das senhas na configuração do roteador

* **Enable secret command**

`enable secret Cisco` : Define uma senha secreta para acessar o modo privilegiado, porém com uma camada extra de segurança

* **Canceling/deleting commands**

`no` : Usado para desativar ou remover uma configuração anterior. Por exemplo, se você usar `no enable secret`, isso removerá a senha secreta definida anteriormente.
