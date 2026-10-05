> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/spanning-tree.md).

# Spanning-Tree

*Spanning Tree Protocol* (referido com o acrónimo STP) é um protocolo para equipamentos de [rede](https://pt.wikipedia.org/wiki/Rede_de_computadores) que permite resolver problemas de *loop* em redes comutadas cuja topologia introduza anéis nas ligações, auxiliando na melhor performance da rede.

O protocolo STP possibilita a inclusão de ligações redundantes entre os [computadores](https://pt.wikipedia.org/wiki/Comutador_\(redes\)), provendo caminhos alternativos no caso de falha de uma dessas ligações.&#x20;

#### Aqui estão seus três fatos críticos sobre Spanning-Tree:

* Deixar STP habilitado, mas não permitir que ele flua por interfaces específicas pode ser uma solução aceitável.

1. Desabilitar STP quase sempre é a solução errada.

* STP existe para proteger sua rede de loops.
* Ser protegido de loops vale o custo de lidar com o mal.
* Estabilidade e previsibilidade são mais importantes que velocidade.

2. STP é necessário.

* STP quer cortar metade da sua largura de banda.

3. STP é mal.

Sempre tente construir triângulos com seus switches. Tente não construir quadrados.

O Switch A é seu bridge raiz STP. O Switch B é sua raiz alternativa. O Switch C deve, como parte de um bom design, estar conectado direta e fisicamente a A e B.

Conectar C a A e Switch D a B e então conectar C a D cria um quadrado e não um triângulo. Isso pode funcionar. Isso vai funcionar. Mas esta é uma configuração menos desejável e deve ser evitada sempre que possível.

***

Prioridades STP válidas são de 0 a 65536. Muito poucos switches permitem que você use o valor "0". A maioria, se não todos, permitirá que você use 4096. Você será tentado a tornar seu bridge raiz 4096. Não faça isso.

Mantenha 4096 no seu bolso para um dia chuvoso. Só por precaução. Um dia você pode precisar mover sua raiz para um novo switch como parte de um processo de atualização. Ter 4096 disponível facilitará esse processo.

* **Então defina sua raiz para 8192 para todas as VLANs, como esta**

```
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1-4094 priority 8192
```

* **Você quer que sua raiz alternativa pretendida seja o próximo valor mais baixo, que é 8192+4096=12288**

```
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1-4094 priority 12288
```

* Agora você quer definir cada switch que está conectado direta e fisicamente (usando um triângulo) ao seu A e B para o próximo valor mais baixo (12288+4096=16384).

```
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1-4094 priority 16384
```

* **Agora você quer que cada switch que esteja conectado a um de seus dispositivos 16384 use o próximo valor mais baixo (16384+4096=20480)**

```
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1-4094 priority 20480
```

Seu objetivo aqui é tentar manter sua topologia de switch definida para valores STP mais baixos que o valor padrão fora da caixa, que é 32768. Dessa forma, se (quando?) algum idiota tirar um novo dispositivo habilitado para STP da caixa e conectá-lo à sua rede, toda a sua rede deve ter uma prioridade STP mais baixa, evitando assim qualquer tipo de mudança de topologia.

Seu próximo objetivo é IMPOR uma falha e reconvergência PREVISÍVEL de sua topologia no caso de um ou mais switches falharem.

Se um de seus dispositivos 16384 falhar, há um caminho muito claro para todos esses dispositivos 20480 encontrarem seu caminho para a raiz. Se a raiz é 8192, mas todo o resto da rede é 32768 (padrão), a reconvergência leva mais tempo.

***

**BPDUGuard** é amor. BPDUGuard é vida. BPDUGuard não é uma mentira - é bolo.

BPDUGuard é um recurso de segurança de borda que defende a borda da sua rede de todas as formas de mudança de Spanning-Tree estrangeira e não planejada.

Qualquer implementação STP que não esteja usando BPDUGuard na borda do usuário está, na minha opinião, errada.

```
panning-tree portfast default
spanning-tree portfast bpduguard default
```

BPDUGuard defenderá sua rede das tempestades de broadcast que ocorrem quando um usuário conecta ambas as portas de um switch Linksys não compatível com STP à sua LAN gerenciada. O Linksys burro não entende STP. Ele não participará de nenhuma detecção de loop. Mas ele passará os quadros de descoberta BPDU do seu dispositivo LAN diretamente, como um broadcast padrão, e eles serão detectados pelo mesmo dispositivo LAN gerenciado. Seu switch se perguntará: "Por que de repente consigo ouvir minha própria voz?" e a resposta imediata será `err-disable`desligar a(s) porta(s) do switch envolvida(s) no loop. Isso frustra o usuário que não consegue entender por que seu switch Linksys não está funcionando. Mas também defende o resto da sua rede do evento de tempestade de broadcast.

***

Rapid Per VLAN Spanning-Tree (RPVST) é (na minha opinião / na minha experiência) o modo STP preferido até cerca de 250 VLANs. Depois de exceder esse nível, é hora de Multiple Spanning-Tree (MST)
