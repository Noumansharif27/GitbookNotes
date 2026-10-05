> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/nfs.md).

# NFS

`Network File System`( `NFS`) é um sistema de arquivos de rede desenvolvido pela Sun Microsystems e tem o mesmo propósito que o SMB. Seu propósito é acessar sistemas de arquivos em uma rede como se fossem locais.&#x20;

* Ao fazer footprinting do NFS, as portas TCP `111`e `2049`são essenciais. Também podemos obter informações sobre o serviço NFS e o host via RPC, conforme mostrado abaixo no exemplo.
* **Nmap**

```sh
sudo nmap <FQDN/IP> -p111,2049 -sV -sC
```

* O `rpcinfo`script NSE recupera uma lista de todos os serviços RPC em execução no momento, seus nomes e descrições, e as portas que eles usam.

```sh
sudo nmap --script nfs* <FQDN/IP> -sV -p111,2049
```

* **Mostrar ações NFS disponíveis**

```sh
showmount -e <FQDN/IP>
```

* **Montagem de compartilhamento NFS**

{% code lineNumbers="true" %}

```sh
mkdir target-NFS
sudo mount -t nfs <FQDN/IP>:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
```

{% endcode %}

* **Listar conteúdo com nomes de usuários e nomes de grupos**

```sh
ls -l mnt/nfs/
```

* **Listar conteúdo com UIDs e GUIDs**

```sh
ls -n mnt/nfs/
```

* Depois de realizar todas as etapas necessárias e obter as informações necessárias, podemos desmontar o compartilhamento NFS.

```sh
cd ..
sudo umount ./target-NFS
```
