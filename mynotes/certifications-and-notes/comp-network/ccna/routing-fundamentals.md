> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/ccna/routing-fundamentals.md).

# Routing Fundamentals

* **Roteamento**

É o processo que os roteadores usam para determinar o caminho que os pacotes IP devem seguir em uma rede para chegar ao seu destino. E os roteadores armazenam rotas para todos os destinos em uma tabela de roteamento.

* **Roteamento dinâmico**

É um processo utilizado em redes de computadores para determinar a melhor rota para o envio de pacotes de dados entre diferentes dispositivos.

* **Roteamento estático**

O roteamento estático é um caminho configurado manualmente pelo qual um pacote deve trafegar para alcançar um destino.

* **Routing Table**

`show ip route` : Exibe a tabela de roteamento do roteador&#x20;

* **Static Routes Configuration**

```
ip route ip-address netmask next-hop

example

ip route 192.168.4.0 255.255.255.0 192.168.13.3
```

* **Static Routes configuration (interface de saída)**

```
ip route ip-address netmask next-hop
ip route ip-address netmask exit-interface next-hop 

example

ip route 192.168.4.0 255.255.255.0 g0/0
```

* **Default Route**

A rota padrão, ou **default route**, é uma configuração em roteadores que direciona o tráfego para um destino que não está especificamente listado nas rotas conhecidas do roteador.

```
ip route 0.0.0.0 0.0.0.0 203.0.113.2

do show ip route
```
