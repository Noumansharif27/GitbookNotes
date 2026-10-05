> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/hcia/enderecamento-ip.md).

# Endereçamento IP

### Comandos básicos de configuração de endereço IP

* **Entre na interface view**

```
[Huawei] interface interface-type interface-number
```

Você pode executar este comando para entrar na view de uma interface especificada e configurar atributos para a interface

* **Configure um endereço IP para uma interface**

```
[Huawei-GigabitEthernet0/0/1] ip address ip-address { mask | mask-length }
```

* **Configure um endereço IP para uma interface lógica**

```
RTA] interface LoopBack 0
[RTA-LoopBack0] ip address 1.1.1.1 255.255.255.255
Ou,
[RTA-LoopBack0] ip address 1.1.1.1 32
```
