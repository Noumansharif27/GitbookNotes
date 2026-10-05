> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/vlan/native-vlan-on-a-router.md).

# Native VLAN on a Router

A **VLAN Nativa** em um roteador se refere à VLAN padrão usada em uma porta **trunk** (de enlace) que transmite tráfego de múltiplas VLANs. A principal característica da VLAN nativa é que os quadros dessa VLAN são **não etiquetados** quando enviados pela interface trunk, enquanto os quadros das outras VLANs são etiquetados com a tag 802.1Q

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FUaqbXrwLbIuG9Yw5ALjt%2Fimage.png?alt=media&amp;token=5ea55f90-95bc-4c2f-9ed8-e7488ffbfa78" alt=""><figcaption></figcaption></figure>

* Vamos redefinir todos os SWITCHES (SW1 e SW2) para a vlan nativa 10

**SW1**

```
int g0/0
switchport trunk native vlan 10
```

**SW2**

```
int g0/0
switchport trunk native vlan 10
int g0/1
switchport trunk native vlan 10
```

**Existem DOIS métodos para configurar a VLAN nativa em um roteador:**

* Use o comando “encapsulation dot1q” em uma subinterface

```
int g0/0.10
encapsulation dot1q 10 native
```

**OU**

* Configure o endereço IP para a VLAN nativa na interface física do roteador (o comando “ **encapsulation dot1q** não é necessário”

```
no interface g0/0.10
interface g0/0
ip address 192.168.1.62 255.255.255.192
```

`show running-config` : para exibir as configurações da interface&#x20;

#### Switch multicada (L3)

* Um SWITCH MULTICAMADA é capaz de COMUTAR e ROTEAR
* É CYBERWARE DA CAMADA 3
* Você pode atribuir endereços IP à sua interface virtual L3, como um roteador
* Você pode criar interfaces virtuais para cada VLAN e atribuir endereços IP a essas interfaces
* Você pode configurar rotas nele, assim como um ROUTER
* Pode ser usado para roteamento entre VLANs
