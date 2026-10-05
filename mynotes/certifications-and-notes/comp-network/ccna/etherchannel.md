> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/etherchannel.md).

# Etherchannel

A agregação de links ou Etherchannel é a utilização de um protocolo para fazer com que vários links se comportem como se fosse apenas uma porta lógica agregada, por exemplo, juntando 4 links de 100Mbps entre dois switches você teria um link de 400Mbps como se fosse apenas uma porta ligada entre os equipamentos.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F5k9wGIFqrUrhlu1dkhho%2Fimage.png?alt=media&amp;token=0f56b8d6-7045-484a-a612-f81566fbd1eb" alt=""><figcaption></figcaption></figure>

* Para o STP ou RSTP é como se o link agregado fosse uma única porta, pois ele passa a enxergar apenas a porta lógica agregada e não mais as portas físicas individuais que formam o link agregado.
* Portanto o etherchannel é um recurso que tem como objetivo agregar segmentos ethernet paralelos em uma única interface, possibilitando balanceamento de carga e redundância livre de loops.
* Com o etherchannel as interfaces físicas são associadas em grupos chamados “channel-groups”, e cada um desses grupos formará uma interface lógica chamada “port-channel”, que irá distribuir o tráfego entre as portas físicas agregadas ao grupo.

Os switches verificam os seguintes parâmetros dos seus vizinhos:

·         Mesma velocidade (Speed);

·         Mesmo modo Duplex;

·         Estado operacional do trunk, ou seja, todas as portas devem ser de acesso ou trunk, não pode misturar o estado operacional;

·         Se a porta é de acesso, todas devem pertencer a mesma VLAN;

·         Se for porta trunk, a lista de VLANs permitidas deve ser a mesma no comando switchport trunk allowed;

·         Ainda em portas trunk, a VLAN nativa deve ser a mesma em todas as interfaces;

·         Configurações do STP nas interfaces devem bater.

COMANDOS APRENDIDOS NESTE CAPÍTULO

* **Configura o método de balanceamento de carga EtherChannel em um SWITCH**

```
SW(config) port-channel load-balance *mode*
```

* **Exibe informações sobre as configurações de balanceamento de carga**

```
SW# show etherchannel load-balance
```

* **Configura uma interface para ser PARTE de um EtherChannel**

```
SW(config-if)# channel-group *number* mode {desirable | auto | active | passive | on}
```

* **Exibe um resumo de EtherChannels em um SWITCH**

```
SW# show etherchannel summary
```

* **Exibe informações sobre as interfaces de canal de porta virtual em um SWITCH**

```
SW# show etherchannel port-channel
```
