> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/hcia/vlan.md).

# VLAN

### O que é uma VLAN

* **VLAN** é a sigla para **Virtual Local Area Network**, ou **Rede Local Virtual** em português. Trata-se de uma tecnologia que permite segmentar uma rede física em várias redes lógicas independentes.
* &#x20;Em outras palavras, com VLANs, é possível criar múltiplas redes locais virtuais dentro de um mesmo switch ou conjunto de switches, sem a necessidade de hardware adiciona.

### VLAN Tag

* O **IEEE 802.1Q** define uma tag VLAN de 4 bytes para quadros Ethernet, permitindo que os switches identifiquem as VLANs às quais os quadros recebidos pertencem

### Atribuição de VLAN baseada em interface

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FtR89Yw3JivGm1ZmM85xU%2FCaptura%20de%20tela%202024-10-30%20202541.png?alt=media&amp;token=f8fad42d-0a10-4485-b860-40e51fb81572" alt=""><figcaption></figcaption></figure>

#### Princípios

* As VLANs são atribuídas com base em interfaces
* Um administrador de rede pré-configura um PVID para cada interface de switch e atribui cada interface a uma VLAN correspondente ao PVID
* Depois que uma interface recebe um quadro não etiquetado, o switch adiciona uma etiqueta carregando o PVID da interface ao quadro. O quadro é então transmitido na VLAN especificada

#### Port Default VLAN ID: PVID

* ID de VLAN padrão para uma interface
* Intervalo de valores: 1–4094

### Atribuição de VLAN baseada em endereço MAC

Mapeamento entre endereços MAC e IDs VLAN no SW1

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FGxJMMW54vFApBhEw8GcS%2FCaptura%20de%20tela%202024-10-30%20203123.png?alt=media&amp;token=f45811ba-e153-4d2f-9ff2-22e2b94e4c98" alt=""><figcaption></figcaption></figure>

#### Princípios

* As VLANs são atribuídas com base nos endereços MAC de origem dos quadros
* Um administrador de rede pré-configura o mapeamento entre endereços MAC e IDs de VLAN
* Depois de receber um quadro não marcado, um switch adiciona a tag VLAN mapeando o endereço MAC de origem do quadro ao quadro. O quadro é então transmitido na VLAN especificada

#### Tabela de mapeamento

* Registra o mapeamento entre endereços MAC e IDs VLAN

### Tipos de interface Ethernet da camada 2

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fy19RdZq7UQLg0x03sPeH%2FCaptura%20de%20tela%202024-10-30%20211053.png?alt=media&amp;token=fb82dcbd-57aa-4eb9-bea7-74a4f097d9ce" alt=""><figcaption></figcaption></figure>

#### Interface de acesso

Uma interface de acesso é usada para conectar um switch a um terminal, como um PC ou servidor. Em geral, os NICs em tal terminal recebem e enviam apenas quadros não marcados. Uma interface de acesso pode ser adicionada a apenas uma VLAN

#### Interface do tronco

Uma interface tronco permite que quadros que pertencem a várias VLANs passem e diferencie os quadros usando a tag 802.1Q. Esse tipo de interface é usado para conectar um switch a outro switch ou a uma subinterface em um dispositivo, como um roteador ou firewall.

#### Interface híbrida

Semelhante a uma interface tronco, uma interface híbrida também permite que quadros que pertencem a várias VLANs passem e diferencie os quadros usando a tag 802.1Q. Você pode determinar se permite que uma interface híbrida carregue tags VLAN ao enviar os quadros de uma ou mais VLANs

### Comandos Básicos de Configuração de VLAN

* **Crie uma ou mais VLANs**
* **Este comando cria uma VLAN e exibe a VLAN view. Se a VLAN a ser criada já existir, este comando exibirá diretamente a VLAN view**

```
[Huawei] vlan vlan-id
```

* **O valor de vlan-id é um número inteiro que varia de 1 a 4094**

```
[Huawei] vlan batch { vlan-id1 [ to vlan-id2 ] }
```

• vlan-id1: especifica um ID de VLAN de início.

&#x20;• vlan-id2: especifica um ID de VLAN final.

### Comandos Básicos de Configuração da Interface de Acesso

* **Define o tipo de link de uma interface**
* **Na interface view, define o tipo de link da interface a ser acessada**

```
[Huawei-GigabitEthernet0/0/1] port link-type access
```

* **Configura uma VLAN padrão para a interface de acesso**

```
[Huawei-GigabitEthernet0/0/1] port default vlan vlan-id
```

### Comandos Básicos de Configuração da Interface do Tronco

* **Define o tipo de link de uma interface**
* **Na interface view, define o tipo de link da interface como tronco**

```
[Huawei-GigabitEthernet0/0/1] port link-type trunk
```

* **Adiciona a interface do tronco às VLANs especificadas**

```
[Huawei-GigabitEthernet0/0/1] port trunk allow-pass vlan { { vlan-id1 [ to vlan-id2 ] } | all }
```

* **(Opcional) Configure uma VLAN padrão para a interface do tronco**

```
[Huawei-GigabitEthernet0/0/1] port trunk pvid vlan vlan-id
```

### Comandos básicos de configuração de interface híbrida

* **Na interface view, define o tipo de link da interface como hybrid**

```
[Huawei-GigabitEthernet0/0/1] port link-type hybrid
```

* **Adicione a interface híbrida às VLANs especificadas**

```
[Huawei-GigabitEthernet0/0/1] port hybrid untagged vlan { { vlan-id1 [ to vlan-id2 ] } | all }
```

* Na interface view, adicione a interface híbrida às VLANs especificadas no modo marcado.

```
[Huawei-GigabitEthernet0/0/1] port hybrid tagged vlan { { vlan-id1 [ to vlan-id2 ] } | all }
```
