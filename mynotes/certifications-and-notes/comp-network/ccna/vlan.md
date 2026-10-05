> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/vlan.md).

# VLAN

**LAN** É um domínio de transmissão único

* **VLAN 0, 4095:** Estas são VLAN reservadas que não podem ser vistas ou usadas.
* **VLAN 1:** É a VLAN padrão dos switches. Por padrão, todas as portas do switch estão em VLAN. Essa VLAN não pode ser excluída ou editada, mas pode ser usada.
* **VLAN 2-1001:** Este é um intervalo VLAN normal. Podemos criar, editar e excluir essas VLANs.
* **VLAN 1002-1005:** Esses são os padrões da CISCO para fddi e anéis de token. Essas VLANs não podem ser excluídas.
* **Vlan 1006-4094:** Este é o alcance estendido da Vlan

**Configuração**

* Exibir as vlans existentes no switch

```
show vlan brief
```

* Criar uma vlan com o id e dar um nome

{% code lineNumbers="true" %}

```
vlan 2
name myvlan_1
```

{% endcode %}

* Atribuir Vlan às portas do switch

{% code lineNumbers="true" %}

```
int fa0/0
switchport mode access
switchport access Vlan 2
```

{% endcode %}

* Além disso, o intervalo de portas de switch pode ser atribuído às vlans necessárias.

{% code lineNumbers="true" %}

```
int range fa0/0-2
switchport mode access
switchport access Vlan 2
```

{% endcode %}
