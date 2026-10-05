> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/smb.md).

# SMB

`Server Message Block`( `SMB`) é um protocolo cliente-servidor que regula o acesso a arquivos e diretórios inteiros e outros recursos de rede, como impressoras, roteadores ou interfaces liberadas para a rede.

* **Nmap**

```sh
sudo nmap <FQDN/IP> -sV -sC -p139,445
```

* **SMBclient - Conectando ao Compartilhamento**

```sh
smbclient -N -L //<FQDN/IP>
```

* Conectar-se a um compartilhamento

```sh
smbclient //<FQDN/IP>/notes
```

* O Smbclient também nos permite executar comandos do sistema local usando um ponto de exclamação no início ( `!<cmd>`) sem interromper a conexão.

* **RPCclient**

```sh
rpcclient -U "" <FQDN/IP>
```

O `rpcclient`nos oferece muitas requisições diferentes com as quais podemos executar funções específicas no servidor SMB para obter informações.&#x20;

| **Consulta**              | **Descrição**                                                               |
| ------------------------- | --------------------------------------------------------------------------- |
| `srvinfo`                 | Informação do servidor.                                                     |
| `enumdomains`             | Enumere todos os domínios implantados na rede.                              |
| `querydominfo`            | Fornece informações de domínio, servidor e usuário de domínios implantados. |
| `netshareenumall`         | Enumera todos os compartilhamentos disponíveis.                             |
| `netsharegetinfo <share>` | Fornece informações sobre um compartilhamento específico.                   |
| `enumdomusers`            | Enumera todos os usuários do domínio.                                       |
| `queryuser <RID>`         | Fornece informações sobre um usuário específico.                            |

* **Força bruta de RIDs de usuários**

```sh
for i in $(seq 500 1100);do rpcclient -N -U "" <FQDN/IP> -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

* Uma alternativa para isso seria um script Python da [Impacket](https://github.com/SecureAuthCorp/impacket) chamado [samrdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py) .
* **Impacket - Samrdump.py**

```sh
samrdump.py <FQDN/IP>
```

* Informações que podemos obter com `rpcclient`também podem ser obtidas usando outras ferramentas.&#x20;
* **SMBmap**

```shell
smbmap -H <FQDN/IP>
```

* **CrackMapExec**

```sh
crackmapexec smb <FQDN/IP> --shares -u '' -p ''
```

* **Enum4Linux-ng - Instalação**

```sh
git clone https://github.com/cddmp/enum4linux-ng.git
cd enum4linux-ng
pip3 install -r requirements.txt
```

* **Enum4Linux-ng - Enumeração**

```shell
./enum4linux-ng.py <FQDN/IP> -A
```

{% hint style="info" %}
Precisamos usar mais de duas ferramentas para enumeração. Porque pode acontecer que, devido à programação das ferramentas, obtenhamos informações diferentes que temos que verificar manualmente. Portanto, nunca devemos confiar apenas em ferramentas automatizadas onde não sabemos precisamente como foram escritas.
{% endhint %}
