> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/hcia/vrp.md).

# VRP

* O **Versatile Routing Platform** (VRP) é uma plataforma de sistema operacional (OS) universal para produtos datacom da Huawei.

### Command Views

`display version` : Verificar as informações básicas do equipamento

`User view` :  Nesta visualização, você pode verificar o status de execução e as estatísticas de um dispositivo

`system-view` : Este comando é usado para entrar na visualização do sistema a partir da visualização do usuário. A user view é a primeira exibição exibida depois que você faz login em um dispositivo.

`Interface GigabitEthernet 0/0/1` : Este comando é usado para entrar na interface view a partir da System View

`display this` : Exibe a configuração em execução na exibição atual

`ip address 192.168.1.1 24` : Este comando é usado para definir um endereço IP&#x20;

`quit` : Este comando é usado para retornar à view anterior&#x20;

`ospf 1` : Este comando é utilizado para introduzir a view de protocolo a partir da system view

`área 0` : Este comando é usado para entrar na view da área OSPF a partir da view OSP

`return` : Este comando é usado para retornar à user view

### Usando a Ajuda Online da Linha de Comando

`?` : Para obter ajuda total, pressione ? após uma view exibida. O sistema exibirá todos os comandos na view e suas descrições.

`d?`: Para obter ajuda parcial, pressione ? depois de inserir o caractere inicial ou a cadeia de caracteres de um comando. O sistema exibirá todos os comandos que começam com este caractere ou cadeia de caracteres.

### Usando Linhas de Comando undo

`undo` : É um comando desfazer

`undo terminal monitor` : Desativar mensagens no terminal

* **Execute um comando undo para restaurar uma configuração padrão**

<pre><code><strong>&#x3C;Huawei> system-view
</strong>[Huawei] sysname Server
[Server] undo sysname
[Huawei]
</code></pre>

* **Executar um comando undo para desabilitar uma função**

```
<Huawei> system-view
[Huawei] ftp server enable
[Huawei] undo ftp server
```

* **Execute um comando undo para excluir uma configuração**

```
[Huawei]interface g0/0/1
[Huawei-GigabitEthernet0/0/1]ip address 192.168.1.1 24
[Huawei-GigabitEthernet0/0/1]undo ip address
```

### Comandos comuns de operação do sistema de arquivos (1)

* **Verifique o diretório atual**

```
<Huawei>pwd
```

* E**xiba informações sobre arquivos no diretório atual**

```
<Huawei>dir
```

* **Exiba o conteúdo de um arquivo de texto**

```
<Huawei>more
```

* **Altere o diretório de trabalho atual**

```
<Huawei>cd
```

* **Crie um diretório**

```
<Huawei>mkdir
```

### Comandos comuns de operação do sistema de arquivos (2)

* **Exclua um diretório**

```
<Huawei>rmdir
```

* **Copie um arquivo**

```
<Huawei>copy
```

* **Mova um arquivo**

```
<Huawei>move
```

* **Renomeie um arquivo**

```
<Huawei>rename
```

* **Exclua um arquivo**

```
<Huawei>delete
```

* **Restaurar um arquivo excluído**

```
<Huawei>undelete
```

* **Exclua permanentemente um arquivo na lixeira**

```
<Huawei>reset recycle-bin
```

### Comandos Básicos de Configuração (1)

* **Configure um nome de sistema**

```
[Huawei] sysname name
```

* **Configure um relógio do sistema**

```
<Huawei> clock timezone time-zone-name { add | minus } offset
```

Este comando configura um fuso horário local

```
<Huawei> clock datetime [ utc ] HH:MM:SS YYYY-MM-DD
```

Este comando configura a data e hora atuais ou UTC

```
<Huawei> clock daylight-saving-time
```

Este comando configura o horário de verão.

## Comandos Básicos de Configuração (2)

* **Configure um nível de comando**

```
[Huawei] command-privilege level level view view-name command-key
```

Este comando configura um nível para comandos em uma exibição especificada. Os níveis de comando são classificados em visita, monitoramento, configuração e gerenciamento, que são identificados pelos números 0, 1, 2 e 3, respectivamente.

* **Configure o modo de login baseado em senha**

```
[Huawei]user-interface vty 0 4
[Huawei-ui-vty0-4]set authentication password cipher information
```

Este comando user-interface vty mostra o usuário virtual type terminal (VTY) da interface view, e o comando set authentication password configura a senha do modo de autenticação. O sistema suporta a interface de usuário do console e a interface de usuário VTY. A interface de usuário do console é usada para login local e a interface de usuário VTY é usada para login remoto. Por padrão, um dispositivo suporta um máximo de 15 acessos de usuário baseados em VTY simultâneos.

* **Configure os parâmetros da interface do usuário**

```
[Huawei] idle-timeout minutes [ seconds ]
```

Este comando define um período de tempo limite para desconectar da interface do usuário. Se nenhum comando for inserido dentro do período especificado, o sistema derruba a conexão atual. O período de tempo limite padrão é de 10 minutos

### Comandos Básicos de Configuração (3)

* **Configure um endereço IP para uma interface**

```
[Huawei]interface interface-number
[Huawei-interface-number]ip address ip address
```

Este comando configura um endereço IP para uma interface física ou lógica em um dispositivo.

* **Exibir configurações atualmente eficazes**

```
<Huawei>display current-configuration 
```

* **Exibir informações resumidas sobre os endereços IP da interface**

```
[Huawei]display ip interface brief
```

* **Salve um arquivo de configuração**

```
<Huawei>save
```

* **Verifique as configurações salvas**

```
<Huawei>display saved-configuration
```

### Comandos Básicos de Configuração (4)

* **Limpe as configurações salvas**

```
<Huawei>reset saved-configuration
```

* **Verifique os parâmetros de configuração de inicialização do sistema**

```
<Huawei> display startup
```

Este comando exibe o software do sistema para a inicialização atual e próxima, software do sistema de backup, arquivo de configuração, arquivo de licença e arquivo de patch, bem como arquivo de voz.

* &#x20;**Configure o arquivo de configuração para a próxima inicialização**

```
<Huawei>startup saved-configuration configuration-file
```

Durante uma atualização do dispositivo, você pode executar este comando para configurar o dispositivo para carregar o arquivo de configuração especificado para a próxima inicialização

* **Reinicie um dispositivo**

```
<Huawei>reboot
```
