> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/comp-network/hcia/roteamento/roteamento-estatico.md).

# Roteamento Estático

### **Roteamento Estático**

* As rotas estáticas são configuradas manualmente por administradores de rede, têm baixos requisitos de sistema e se aplicam a redes simples, estáveis e pequenas

### Configuração de Rota Estática

* **Especifica um endereço IP de próximo salto para uma rota estática**

```
[Huawei] ip route-static ip-address {mask | mask-length} nexthop-address
```

* **Especifica uma interface de saída para uma rota estática**

```
[Huawei] ip route-static ip-address { mask | mask-length } interface-type interface-number
```

* **Especifica a interface de saída e o próximo salto para uma rota estática**

<pre><code><strong>[Huawei] ip route-static ip-address { mask | mask-length } interface-type interface-number [ nexthop-address ]
</strong></code></pre>

* **Exibir a tabela de roteamento**

```
[Huawei] display ip routing-table
```

### Exemplo de configuração

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F5M7aHABBDhK0RZ7ATJFQ%2FCaptura%20de%20tela%202024-10-27%20120154.png?alt=media&amp;token=d51dd1eb-6f68-4dea-972d-0cc353900d2a" alt=""><figcaption></figcaption></figure>

* **Configure o RTA**

```
[RTA] ip route-static 20.1.1.0 255.255.255.0 10.0.0.2 
```

* **Configure o RTC**

```
[RTC] ip route-static 10.0.0.0 255.255.255.0 S1/0/0
```

* Configurar rotas estáticas em RTA e RTC para comunicação entre 10.0.0.0/24 e 20.1.1.0/24
* Os pacotes são encaminhados salto por salto. Portanto, todos os roteadores ao longo do caminho desde a origem até o destino devem ter rotas destinadas ao destino
* A comunicação de dados é bidirecional. Portanto, as rotas para frente e para trás devem estar disponíveis
