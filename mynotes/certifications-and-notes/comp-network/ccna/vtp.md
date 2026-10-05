> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/vtp.md).

# VTP

#### VTP (Protocolo de Trunk VLAN)

O protocolo VLAN Trunk (VTP) reduz a administração em uma rede comutada. Quando você configura um VLAN novo em um servidor VTP, o VLAN é distribuído por meio de todos os switches no domínio. Isso reduz a necessidade de configurar a mesma VLAN em todos os lugares.&#x20;

* Protocolo para configurar VLANs em um SWITCH Central
  * Um SERVIDOR que outros SWITCHES sincronizam. para (configuração automática por conexão)
* Outros switches (VTP CLIENTS) sincronizarão seu banco de dados VLAN com o SERVIDOR
* Projetado para redes grandes com muitas VLANs (reduz a configuração manual)
* RARAMENTE usado. Recomendado que você NÃO USE
* Existem TRÊS versões de VTP:
  * v1
    * NÃO suporta o intervalo de VLAN estendido 1006-4094
  * v2
    * NÃO suporta o intervalo de VLAN estendido 1006-4094
    * Suporta VLANs Token Ring; caso contrário, semelhante a V1
  * v3
    * Suporta faixa VLAN estendida 1006-4094
    * CLIENTES armazenam VLAN dBase na NVRAM
* Existem **TRÊS modos VTP**:
  * SERVIDOR
  * CLIENTE
  * TRANSPARENTE
* Os Cisco SWITCHES operam no modo VTP SERVER, por padrão

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FJVBoXPWfbk3ZX06tb8gQ%2Fimage.png?alt=media&amp;token=607c8e95-4028-40ea-88d7-26d06100f227" alt=""><figcaption></figcaption></figure>

A configuração do VTP é muito mais simples que sua teoria, basicamente temos que definir os seguintes itens:

**Mostrar as configurações do `VTP (mode privi)`**

```
show vtp status
```

**1)   Versão do VTP:**

<pre><code><strong>SW(config)#vtp  version  {1 | 2 | 3}
</strong></code></pre>

**2)   Nome do domínio:**

```
vtp  domain  nome-do-dominio
```

**3)   Modo de operação:**

```
vtp mode {server  |  client  | transparent | off}
```

A opção “vtp mode off” está disponível somente se o switch suportar o VTP versão 3. Para seus laboratórios simplesmente entre com o comando “vtp mode transparent” nos switches antes de iniciar as configurações de VLANs e Trunks.

**4)   Senha do domínio:**

```
vtp  password  senha [ hidden | secret ]
```

As opções `hidden` e `secret` na configuração da senha do domínio podem não estar disponíveis em versões de Cisco IOS mais antigas.

**5)   Pruning ou filtragem automática de VLANs nos trunks:**

```
vtp  pruning
```

{% hint style="warning" %}
Para não usar o VTP basta colocar o switch em modo transparente ou OFF
{% endhint %}
