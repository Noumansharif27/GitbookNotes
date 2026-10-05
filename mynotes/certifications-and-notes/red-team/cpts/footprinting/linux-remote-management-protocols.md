> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/linux-remote-management-protocols.md).

# Linux Remote Management Protocols

#### SSH <a href="#ssh" id="ssh"></a>

[O Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) ( SSH `SSH`) permite que dois computadores estabeleçam uma conexão criptografada e direta em uma rede possivelmente insegura utilizando a porta padrão `tcp 22`.&#x20;

* **SSH-Audit**

{% code lineNumbers="true" %}

```
git clone https://github.com/jtesta/ssh-audit.git && cd ssh-audit
./ssh-audit.py 10.129.14.132
```

{% endcode %}

* **Change Authentication Method**

{% code lineNumbers="true" %}

```
ssh -v user@10.129.14.132
ssh -v user@10.129.14.132 -o PreferredAuthentications=password
```

{% endcode %}

#### Rsync <a href="#rsync" id="rsync"></a>

[O Rsync](https://linux.die.net/man/1/rsync) é uma ferramenta rápida e eficiente para copiar arquivos localmente e remotamente.Por padrão, ele usa a porta `873`&#x20;

* **Verificando se há Rsync**

```
sudo nmap -sV -p 873 127.0.0.1
```

* Em seguida, podemos explorar um pouco o serviço para ver a que podemos ter acesso.

```
nc -nv 127.0.0.1 873
```

* **Enumerando uma ação aberta**

```
rsync -av --list-only rsync://127.0.0.1/dev
```
