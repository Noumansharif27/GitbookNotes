> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/switch-interfaces.md).

# Switch Interfaces

* **Switchs**

São usados para conectar ou interligar vários hosts na mesma rede

* **Show command**

`show interfaces status` : Ver o estado de cada porta do switch

* **Configuring interface speed and duplex**

{% code lineNumbers="true" %}

```
conf t
int f0/1
speed 100
duplex full
desc ## to R1 ##

other interfaces

desc ## to SW2 ##
desc ## to end hosts ##
```

{% endcode %}

* **Interface** **range**

{% code lineNumbers="true" %}

```
inferface range f0/5 - 12
desc ## not in use ##
shutdown

or 

int range f0/5 - 6, f0/9 - 12
no shutdown
```

{% endcode %}

* **Half** **duplex**&#x20;

Signfica que o despositivo não pode enviar e receber dados ao mesmo tempo

* **Full** **duplex**

Significa que o despositivo pode enviar e receber dados ao mesmo tempo
