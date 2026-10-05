> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/cisco-modeling-labs/labs-with-5-nodes.md).

# Labs With 5 nodes

{% embed url="<https://www.youtube.com/watch?v=FrB5tyYrGMQ>" %}

#### Fase 1: Fundamentos da CLI, Switches e VLANs

<details>

<summary><mark style="color:orange;">Configuração básica de router e switch</mark></summary>

* **Objetivo**: Nomear dispositivos, configurar senhas, banners, desativar DNS, ativar interfaces.
* **Passos**:

1. *Conectar um roteador e um switch.*
2. *Acessar modo privilegiado e global.*
3. *Criar senha enable, linhas VTY e console.*
4. *Criar banner de aviso e hostname.*

</details>

<details>

<summary><mark style="color:orange;">VLANs e trunking</mark></summary>

* **Objetivo:** Criar VLANs, atribuir portas a VLANs, configurar trunk entre switches.
* **Passos**:

1. *Criar VLANs 10 e 20 nos switches.*
2. *Atribuir portas específicas a essas VLANs.*
3. *Configurar trunk entre dois switches.*

</details>

<details>

<summary><mark style="color:orange;">Inter-VLAN Routing (Router-on-a-stick)</mark></summary>

* **Objetivo**: Permitir comunicação entre VLANs via subinterfaces em um roteador.
* **Passos**:

1. *Conectar roteador a um switch com trunk.*
2. *Criar subinterfaces com encapsulamento dot1Q.*
3. *Atribuir IPs para VLANs.*

</details>

<details>

<summary><mark style="color:orange;">Port Security</mark></summary>

* **Objetivo**: Restringir acesso à porta com base no MAC address.
* **Passos**:

1. *Ativar switchport security.*
2. *Definir número máximo de MACs.*
3. *Testar comportamento com outro host.*

</details>

<details>

<summary><mark style="color:orange;">CDP e LLDP</mark></summary>

* Objetivo: Visualizar vizinhos conectados.
* Passos:

1. *Usar show cdp neighbors.*
2. *Desativar CDP em interfaces específicas.*
3. *Habilitar e testar LLDP.*

</details>

#### Fase 2: Roteamento IPv4 e IPv6

<details>

<summary><mark style="color:orange;">Roteamento Estático</mark></summary>

* **Objetivo**: Configurar rotas manuais entre redes diferentes.
* **Passos**:

1. *Conectar 2 roteadores.*
2. *Criar redes locais em cada ponta.*
3. *Criar rotas estáticas apontando para o próximo salto.*

</details>

<details>

<summary><mark style="color:orange;">RIP v2</mark></summary>

* **Objetivo**: Configurar roteamento dinâmico RIP com múltiplas redes.
* **Passos**:

1. *Ativar RIP nos roteadores.*
2. *Anunciar redes conectadas.*
3. *Verificar aprendizado de rotas.*

</details>

<details>

<summary> <mark style="color:orange;">OSPF (Single Area)</mark></summary>

* **Objetivo**: Configurar OSPF em área 0.
* **Passos**:

1. *Ativar OSPF com process ID.*
2. *Anunciar redes com wildcard mask.*
3. *Verificar neighbors e tabela de roteamento.*

</details>

<details>

<summary><mark style="color:orange;">OSPF (Multi-Area)</mark></summary>

* **Objetivo**: Dividir rede OSPF em múltiplas áreas.
* **Passos**:

1. *Designar áreas 0 e 1.*
2. *Identificar ABRs.*
3. *Verificar tabela de roteamento inter-área.*

</details>

<details>

<summary><mark style="color:orange;">EIGRP</mark></summary>

* **Objetivo**: Configurar roteamento dinâmico com EIGRP.
* **Passos**:

1. *Ativar EIGRP com AS number.*
2. *Anunciar redes.*
3. *Verificar neighbors e métricas.*

</details>

#### Fase 3: IPv6, ACLs, NAT, DHCP

<details>

<summary><mark style="color:orange;">IPv6 Routing &#x26; OSPFv3</mark></summary>

* **Objetivo**: Trabalhar com IPv6 em roteadores.
* **Passos**:

1. *Atribuir endereços IPv6 às interfaces.*
2. *Ativar roteamento IPv6.*
3. *Configurar OSPFv3.*

</details>

<details>

<summary><mark style="color:orange;">ACLs (Padrão e Estendida)</mark></summary>

* **Objetivo**: Restringir tráfego com base em IP e portas.
* **Passos**:

1. *Criar ACL padrão e aplicar em interface de saída.*
2. *Criar ACL estendida para bloquear acesso HTTP, etc.*

</details>

<details>

<summary><mark style="color:orange;">DHCP (Server)</mark></summary>

* **Objetivo**: Usar roteador como servidor DHCP.
* **Passos**:

1. *Criar pool de IP.*
2. *Definir default gateway e DNS.*
3. *Excluir IPs do escopo.*

</details>

<details>

<summary><mark style="color:orange;">NAT (Estático, Dinâmico e PAT)</mark></summary>

* **Objetivo**: Traduzir endereços privados para públicos.
* **Passos**:

1. *Criar pools de IPs.*
2. *Configurar NAT estático para host único.*
3. *Configurar overload para PAT.*

</details>

<details>

<summary><mark style="color:orange;">Syslog e NTP</mark></summary>

• **Objetivo**: Monitorar eventos e sincronizar relógios.

• **Passos**:

1. *Definir servidor syslog.*
2. *Configurar cliente e servidor NTP.*

</details>

#### Fase 4: Switching Avançado e Redundância

<details>

<summary><mark style="color:orange;">STP e Root Bridge</mark></summary>

• **Objetivo**: Evitar loops com Spanning Tree.

• **Passos**:

1. *Forçar bridge root com prioridade menor.*
2. *Verificar portas root/designated.*

</details>

<details>

<summary><mark style="color:orange;">EtherChannel</mark></summary>

* **Objetivo**: Agregar múltiplos links entre switches.
* **Passos**:

1. *Criar channel-group em ambos os lados.*
2. *Testar conectividade e redundância.*

</details>

<details>

<summary><mark style="color:orange;">VTP</mark></summary>

* **Objetivo**: Propagar VLANs entre switches.
* **Passos**:

1. *Definir domínio VTP.*
2. *Configurar switches como client/server.*

</details>

<details>

<summary><mark style="color:orange;">HSRP (Failover entre roteadores)</mark></summary>

* **Objetivo**: Configurar gateway virtual para redundância.
* **Passos**:

1. *Configurar IP virtual em dois roteadores.*
2. *Definir prioridade e preempt.*
3. *Testar failover com pings.*

</details>

#### Fase 5: WAN, Túnel e Segurança Avançada

<details>

<summary><mark style="color:orange;">PPP (PAP e CHAP)</mark></summary>

* **Objetivo**: Estabelecer link ponto-a-ponto autenticado.
* **Passos**:

1. *Configurar encapsulamento PPP.*
2. *Criar autenticação PAP/CHAP.*

</details>

<details>

<summary><mark style="color:orange;">GRE Tunnel</mark></summary>

* **Objetivo**: Estabelecer túnel privado sobre rede pública.
* **Passos**:

1. *Criar interfaces tunnel.*
2. *Encaminhar rotas através do túnel.*

</details>

<details>

<summary><mark style="color:orange;">BGP (Simples)</mark></summary>

* **Objetivo**: Simular roteamento entre ASs.
* **Passos**:

1. *Definir número AS.*
2. *Estabelecer vizinhança com outro roteador.*
3. *Anunciar prefixos e verificar troca de rotas.*

</details>
