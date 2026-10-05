> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/connecting-to-target.md).

# Connecting to Target

* Ferramenta baseada em CLI usada para conectar a um destino do Windows usando o Protocolo de Área de Trabalho Remota.

```sh
xfreerdp /v:<ip> /u:htb-student /p:HTB_@cademy_stdnt!
```

* `Evil-WinRM` para estabelecer uma sessão do Powershell com um alvo.

```sh
evil-winrm -i <ip> -u user -p password
```

* `SSH` para se conectar a um alvo usando um usuário especificado.

```sh
ssh user@<ip>
```

* `smbclient` para se conectar a um compartilhamento SMB usando um usuário especificado.

```sh
smbclient -U user \\\\<ip>\\SHARENAME
```

* `smbserver.py` para criar um compartilhamento em um host de ataque baseado em Linux. Pode ser útil quando precisar transferir arquivos de um alvo para um host de ataque.

```sh
python3 smbserver.py -smb2support CompData /home/<nameofuser>/Documents/
```
