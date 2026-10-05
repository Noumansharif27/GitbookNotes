> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/protocolos-cdp-e-lldp.md).

# Protocolos CDP e LLDP

**PROTOCOLO DE DESCOBERTA DA CISCO (CDP)**

* O CDP é um protocolo proprietário da Cisco
* Ele é habilitado em dispositivos Cisco (roteadores, switches, firewalls, telefones IP, etc.) por PADRÃO

{% hint style="info" %}
As mensagens CDP são enviadas periodicamente para o ENDEREÇO MAC multicast '0100.0CCC. CCCC'
{% endhint %}

* Quando um DISPOSITIVO recebe uma mensagem CDP, ele PROCESSA e DESCARTA a mensagem. Ele NÃO o encaminha para outros dispositivos.
* Por padrão, as mensagens da CDP são enviadas uma vez a cada **60 segundos**
* Por PADRÃO, o tempo de espera do CDP é de **180 segundos.** Se uma mensagem não for recebida de um vizinho por 180 segundos, o vizinho será REMOVIDO da Tabela de Vizinhos da CDP
* As mensagens CDPv2 são enviadas por PADRÃO

Por COMPARTILHAREM INFORMAÇÕES sobre os DISPOSITIVOS na REDE, podem ser considerados um risco de segurança e muitas vezes NÃO são utilizados. Cabe ao ENGENHEIRO DE REDE / ADMIN decidir se deseja usá-los na REDE ou não.

{% hint style="info" %}
Quando uma grande quantidade de anúncios de vizinhos da CDP é enviada, é possível consumir toda a memória de um dispositivo disponível. Isso causa uma falha ou outro comportamento anormal. Consulte [a resposta da Cisco ao problema do CDP](http://www.cisco.com/en/US/tech/tk962/technologies_security_notice09186a0080093ef0.html) para obter mais detalhes:
{% endhint %}

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FuL1J4StxkD4fyZFeMIgs%2Fimage.png?alt=media&amp;token=5eb1f630-280a-4e0a-b625-49e8454968e3" alt=""><figcaption></figcaption></figure>

**COMANDOS DE CONFIGURAÇÃO DO CDP**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FYaPHHnBpPDC2R5vkDltg%2Fimage.png?alt=media&amp;token=8da21335-7d1a-433e-ba87-8e290841ca2a" alt=""><figcaption></figcaption></figure>

***

**PROTOCOLO DE DESCOBERTA DE CAMADA DE ENLACE (LLDP)**

* LLDP é um PROTOCOLO PADRÃO DA INDÚSTRIA (IEEE 802.1AB)
* Geralmente é DESABILITADO em dispositivos Cisco, por PADRÃO, portanto, deve ser HABILITADO manualmente
* Um dispositivo pode executar CDP e LLDP ao mesmo tempo

{% hint style="info" %}
&#x20;As mensagens LLDP são enviadas periodicamente para o ENDEREÇO MAC Multicast '0180.c200.000E'
{% endhint %}

* Quando um DISPOSITIVO recebe uma mensagem LLDP, ele PROCESSA e DESCARTA a mensagem. Ele NÃO o encaminha para OUTROS DISPOSITIVOS
* Por PADRÃO, as mensagens LLDP são enviadas uma vez a cada **30 segundos**
* Por padrão, o tempo de espera do LLDP é de **120 segundos**
* O LLDP tem um temporizador adicional chamado 'atraso de reinicialização'
  * Se o LLDP estiver HABILITADO (globalmente ou em uma INTERFACE), este TEMPORIZADOR atrasará a inicialização real do LLDP (**2 segundos,** por PADRÃO)

***

COMANDOS DE CONFIGURAÇÃO LLDP

* O LLDP geralmente é GLOBALMENTE DESATIVADO por PADRÃO
* O LLDP também está DESABILITADO em cada INTERFACE, por PADRÃO
* PARA HABILITAR O LLDP GLOBALMENTE: `R1(config)# lldp run`
* Para HABILITAR LLDP em INTERFACES específicas (tx): `R1(config-if)# lldp transmit`
* Para HABILITAR LLDP em INTERFACES específicas (rx): `R1(config-if)# lldp receive`

VOCÊ PRECISA HABILITAR AMBOS PARA ENVIAR E RECEBER (a menos que você queira habilitar apenas ENVIAR ou RECEBER mensagens LLDP)

* Configure o temporizador LLDP: `R1(config)# lldp timer *seconds*`
* Configure o tempo de espera do LLDP: `R1(config)# lldp holdtime *seconds*`
* Configure o temporizador de reinicialização do LLDP: `R1(config)# lldp reinit *seconds*`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZ40Ul85K9wkqNdSwTuMl%2Fimage.png?alt=media&amp;token=e52586fe-cfc5-425d-aa30-dfa0e0ac9837" alt=""><figcaption></figcaption></figure>
