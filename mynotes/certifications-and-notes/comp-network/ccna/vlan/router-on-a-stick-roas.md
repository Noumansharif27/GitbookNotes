> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/vlan/router-on-a-stick-roas.md).

# Router on a Stick (ROAS)

Um roteador em um stick é uma das maneiras de permitir o roteamento entre VLANs. Esse tipo de configuração consiste em um roteador e um switch conectados por meio de um link Ethernet configurado como um link tronco 802.1q.

**Subinterfaces** são os elementos lógicos de uma interface física . Graças a essa abordagem, não é necessário usar N interfaces físicas do roteador em N VLANs.

#### **Uma configuração de roteador em um stick**

Suponha que temos uma rede de dois computadores em VLANs diferentes. Temos VLAN 10 e VLAN 20. Para habilitar a comunicação entre PC1 da VLAN 10 e PC2 da VLAN 20, podemos usar uma abordagem de roteador em um stick. A topologia é a seguinte:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Ffx2Xob60KheRce1gkRAZ%2Fimage.png?alt=media&amp;token=54788ddd-997d-4bec-b44d-0fa3492475da" alt=""><figcaption></figcaption></figure>

* Vamos começar configurando a porta que conecta o switch ao roteador. Lembre-se de que a conexão entre o roteador e o switch deve ser definida pelo link trunk:

```
Switch#configure terminal
Switch(config)#int Fa0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#spanning-tree portfast trunk
```

* Então, vamos criar as VLANs necessárias e configurar as portas de acesso para o DTE

```
Switch#configure terminal
Switch(config)#vlan 10
Switch(config)#vlan 20
Switch(config)#int Fa0/2
Switch(config-if)switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit
Switch(config)#int Fa0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
```

* No final, começamos nossa configuração de roteador definindo subinterfaces. Na porta que conecta o roteador com o switch, configuramos subinterfaces para cada VLAN.
* Também definimos o encapsulamento 802.1Q com o número de VLAN ao qual a subinterface pertencerá. As subinterfaces são a instância lógica da porta física Gig0/0 (neste caso). O endereço IP é então atribuído do pool para a VLAN específica

```
Router(config)#interface GigabitEthernet0/0.1
Router(config-subif)#encapsulation dot1q 10
Router(config-subif)#ip address 10.1.10.200 255.255.255.0
Router(config-subif)#interface GigabitEthernet0/0.2
Router(config-subif)#encapsulation dot1q 20
Router(config-subif)#ip address 10.1.20.200 255.255.255.0
Router(config-subif)#int Gig0/0
Router(config-if)#no shutdown
```
