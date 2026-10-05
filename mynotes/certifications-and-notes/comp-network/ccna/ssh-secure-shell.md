> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/ssh-secure-shell.md).

# SSH (Secure Shell)

### Console Port Security

* Por PADRÃO, nenhuma senha é necessária para acessar a CLI de um DISPOSITIVO CISCO IOS por meio da PORTA DO CONSOLE
* Você pode CONFIGURAR uma SENHA na *linha do console*
  * Um USUÁRIO terá que digitar uma SENHA para ACESSAR a CLI via PORTA DO CONSOLE

{% code lineNumbers="true" %}

```
line console 0
password cisco
login
end
exit
```

{% endcode %}

* Como alternativa, você pode configurar a LINHA DO CONSOLE para exigir que os USUÁRIOS façam LOGIN usando um dos NOMES DE USUÁRIO configurados no DISPOSITIVO

```
username savitar secret cisco
line console 0
login local
```

### Layer 2 Switch - Management IP

* SWITCHES DE CAMADA 2 não realizam ROTEAMENTO DE PACOTES e criam uma TABELA DE ROTEAMENTO. Eles NÃO estão cientes de ROTEAMENTO DE IP
* No entanto, você PODE atribuir um ENDEREÇO ​​IP a um SVI para permitir CONEXÕES REMOTAS à CLI do SWITCH (usando Telnet ou SSH)

{% code lineNumbers="true" %}

```
interface vlan 1
ip address 192.168.1.253 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.1.254
```

{% endcode %}

### Telnet

* TELNET (Teletype Network) é um PROTOCOLO usado para ACESSAR REMOTAMENTE a CLI de um HOST REMOTO
* O TELNET foi desenvolvido em 1969
* O TELNET foi amplamente SUBSTITUÍDO pelo SSH, que é MAIS Seguro
* TELNET envia dados em TEXTO SIMPLES. SEM CRIPTOGRAFIA

{% code lineNumbers="true" %}

```powershell
enable secret cisco
username savitar secret cisco
line vty 0 15
login local
transport input telnet #Permitir apenas conexões Telnet nas linhas VTY
```

{% endcode %}

#### Telnet Connect

```
telnet 192.168.1.253
```

### SSH

* O SSH (Secure Shell) foi desenvolvido em 1995 para SUBSTITUIR PROTOCOLOS MENOS SEGUROS, como TELNET
* SSHv2, uma revisão importante do SSHv1, foi lançado em 2006
* Se um DISPOSITIVO suporta v1 e v2, diz-se que ele executa a 'versão 1.99'
* Fornece recursos de SEGURANÇA; como CRIPTOGRAFIA DE DADOS e AUTENTICAÇÃO

**Verifique o suporte SSH**

{% code lineNumbers="true" %}

```powershell
show version # Se aparecer K9 na versão do IOS, significa que suporta o SSH
show ip version
```

{% endcode %}

* **Configuração de um nome de domínio (hostname) e domínio DNS**

{% code lineNumbers="true" %}

```
conf t
hostname SW2
ip domanin name exemplo.com
```

{% endcode %}

* **Gerar as chaves RSA**

```
crypto key generate rsa modulus 2048
```

* **Ativar o SSH nas linhas VTY**

{% code lineNumbers="true" %}

```powershell
line vty 0 4
transport input ssh #Aceitar apenas conexões SSH
login local
```

{% endcode %}

* **Configurar um nome de usuário e senha**

```
username admin privilege 15 secret senhaSegura
```

* **Configurar o modo de autenticação para SSH**

```powershell
ip ssh version 2 #Usar a versão 2 do SSH, é mais segura que
```

#### Habilitar o acesso à interface de gerenciamento (opcional)

{% code lineNumbers="true" %}

```
interface vlan 1
ip address 192.168.1.253 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.1.254
```

{% endcode %}

Após configurar o SSH, você pode verificar se a configuração foi bem-sucedida usando os seguintes comandos

* **Verificar as chaves criptográficas**

```
show crypto key mypubkey rsa
```

* **Verificar as configurações de SSH**

```
show ip ssh
```

* **Verificar as linhas VTY**

```
show running-config | section line vty
```

* #### **Testar a Conexão SSH**

```
ssh admin@192.168.1.10
```

{% embed url="<https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/17-9/configuration_guide/sec/b_179_sec_9300_cg/configuring_secure_shell__ssh_.html>" %}

{% embed url="<https://www.puckiestyle.nl/cisco-password-cracking/>" %}
