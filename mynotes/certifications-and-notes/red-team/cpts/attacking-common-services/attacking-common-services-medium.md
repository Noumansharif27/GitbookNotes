> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/attacking-common-services-medium.md).

# Attacking Common Services - Medium

1. Avalie o servidor de destino e encontre o arquivo flag.txt. Envie o conteúdo deste arquivo como sua resposta.

* **Nmap**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Ff5zRXLplyXVlqbSgnTmz%2Fimage.png?alt=media&amp;token=2b7dcb05-44d0-45f0-b9a6-c206a8c82243" alt=""><figcaption></figcaption></figure>

* Fazer login `anonymous` no ftp na porta `30021`
* Fazer download do arquivo `mynotes`  do diretorio `simon` que supostamente é nome de um usuário
* Notamos que é um arquivo que contém senhas, vamos tentar descobrir uma senha válida

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F0HLOUfyZBHjDPNyM4tcu%2Fimage.png?alt=media&amp;token=0e93c39f-a174-4ced-95c1-c80c258552fd" alt=""><figcaption></figcaption></figure>

* ssh nas credenciais encontradas

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FxS4RpDO7lVrME3LupC2i%2Fimage.png?alt=media&amp;token=62701117-9898-4ce8-b5b8-0b38101abb3e" alt=""><figcaption></figcaption></figure>
