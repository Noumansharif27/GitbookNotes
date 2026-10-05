> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/windows-remote-management-protocols.md).

# Windows Remote Management Protocols

#### RDP <a href="#rdp" id="rdp"></a>

O [Protocolo de Área de Trabalho Remota](https://docs.microsoft.com/en-us/troubleshoot/windows-server/remote/understanding-remote-desktop-protocol) ( `RDP`RDP) é um protocolo desenvolvido pela Microsoft para acesso remoto a um computador com o sistema operacional Windows.O RDP opera na camada de aplicação do modelo de referência TCP/IP, geralmente utilizando a porta TCP 3389.

* **Nmap**

```
nmap -sV -sC 10.129.201.248 -p3389 --script rdp*
```

* **Verificação de segurança RDP**

{% code lineNumbers="true" %}

```
git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check
./rdp-sec-check.pl 10.129.201.248
```

{% endcode %}

* **Iniciar uma sessão RDP**

```
xfreerdp /u:user /p:"P455w0rd!" /v:10.129.201.248
```

#### WinRM <a href="#winrm" id="winrm"></a>

O Windows Remote Management (`WinRM`) é um protocolo simples de gerenciamento remoto integrado ao Windows, baseado na linha de comando.&#x20;

* **Nmap WinRM**

```
nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n
```

* Em ambientes baseados em Linux, podemos usar a ferramenta [evil-winrm](https://github.com/Hackplayers/evil-winrm)&#x20;

```
evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!
```

#### WMI <a href="#wmi" id="wmi"></a>

O WMI permite acesso de leitura e gravação a praticamente todas as configurações em sistemas Windows.

* **WMIexec.py**

```
wmiexec.py user:"P455w0rD!"@10.129.201.248 "hostname"
```
