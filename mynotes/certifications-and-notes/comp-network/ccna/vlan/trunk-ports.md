> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/vlan/trunk-ports.md).

# Trunk Ports

* Em uma rede pequena com poucas VLANS, é possível usar uma interface separada para CADA VLAN ao conectar SWITCHES a SWITCHES e SWITCHES a ROTEADORES
* NO ENTANTO, quando o número de VLANS aumenta, isso não é viável. Isso resultará em interfaces desperdiçadas e, muitas vezes, os ROTEADORES não terão INTERFACES suficientes para cada VLAN
* Você pode usar **TRUNK PORTS** para transportar tráfego de várias VLANS em uma única interface

Uma **PORTA TRUNK** que transporta várias conexões VLAN em uma única interface

**VLAN Tags**

TRUNK PORT = “Tagged” ports

ACCESS PORT = “Untagged” ports

**IEEE 802.1Q**

O padrão [IEEE](https://pt.wikipedia.org/wiki/IEEE) 802.1Q permite a criação de redes virtuais locais ([VLANs](https://pt.wikipedia.org/wiki/Virtual_LAN)) dentro de uma rede [Ethernet](https://pt.wikipedia.org/wiki/Ethernet). A ideia principal é a de adicionar rótulos de 32 bits (802.1Q *tags*) nos quadros Ethernet e instruir os elementos comutadores de [camada de enlace](https://pt.wikipedia.org/wiki/Camada_de_enlace_de_dados) (ex. [*switches*](https://pt.wikipedia.org/wiki/Switches), [*bridges*](https://pt.wikipedia.org/wiki/Bridge_\(redes_de_computadores\))) a trocarem entre si apenas quadros contendo um mesmo identificador.

### Trunk Configuration

* Selecionar a interface para configurar&#x20;

```
interface g0/0
```

* Modo trunk manual

```
switchport mode trunk
```

* Definir o modo de encapsulamento para 802.1Q

```
switchport trunk encapsulation dot1q
```

* Configurar manualmente a interface para trunk

```
switchport mode trunk
```

* Confirmar interfaces no trunk

```
show interfaces trunk
```

{% hint style="info" %}
Para configurar MANUALMENTE a INTERFACE como uma PORTA TRUNK, você deve primeiro definir o encapsulamento para “802.1Q” ou “ISL”. Em SWITCHES que suportam apenas 802.1Q, isso não é necessário
{% endhint %}

* Permitir uma VLAN em um determinado TRUNK

```
switchport trunk allowed vlan 10,30
```

* Por motivos de segurança é melhor alterar a VLAN Nativa para uma VLAN não utilizada

```
switchport trunk native vlan 1001
```

* Adicionar uma VLAN na lista de vlans permitidas de uma porta

```
switchport trunk allowed vlan add 20 
```

* Remover uma VLAN da lista de vlans permitidas a uma porta

```
switchport trunk allowed vlan remove 20
```

* Mostar portas as access ports da VLAN

```
show vlan brief
```

* Mostrar portas trunk

```
show interfaces trunk
```
