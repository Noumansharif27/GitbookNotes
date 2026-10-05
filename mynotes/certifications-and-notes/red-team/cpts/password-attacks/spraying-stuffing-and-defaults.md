> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/spraying-stuffing-and-defaults.md).

# Spraying, Stuffing, and Defaults

**Password spraying**

* É um tipo de ataque no qual um invasor tenta usar uma única senha em diversas contas de usuário diferentes.
* Exemplo

```
netexec smb 10.100.38.0/24 -u <usernames.list> -p 'ChangeMe123!'
```

**Credential stuffing**

* É outro tipo de ataque de força bruta no qual um invasor usa credenciais roubadas de um serviço para tentar obter acesso a outros.
* Exemplo

```
hydra -C user_pass.list ssh://10.100.38.23
```

**Default credentials**

* Muitos sistemas — como roteadores, firewalls e bancos de dados — vêm com credenciais pré-definidas `default credentials`.
* Um exemplo amplamente utilizado é o [Default Credentials Cheat Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet)
