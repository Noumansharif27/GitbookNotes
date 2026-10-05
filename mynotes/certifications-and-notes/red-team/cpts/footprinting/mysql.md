> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/mysql.md).

# MySQL

`MySQL`é um sistema de gerenciamento de banco de dados relacional SQL de código aberto desenvolvido e suportado pela Oracle. Um banco de dados é simplesmente uma coleção estruturada de dados organizados para fácil uso e recuperação.&#x20;

* **Verificando o servidor MySQL**

```sh
sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
```

* Alguns dos comandos que devemos lembrar e anotar para trabalhar com bancos de dados MySQL estão descritos abaixo na tabela.

<table data-header-hidden><thead><tr><th width="392"></th><th></th></tr></thead><tbody><tr><td><strong>Comando</strong></td><td><strong>Descrição</strong></td></tr><tr><td><code>mysql -u &#x3C;user> -p&#x3C;password> -h &#x3C;IP address></code></td><td>Conecte-se ao servidor MySQL. <strong>Não</strong> deve haver espaço entre o sinalizador '-p' e a senha.</td></tr><tr><td><code>show databases;</code></td><td>Mostrar todos os bancos de dados.</td></tr><tr><td><code>use &#x3C;database>;</code></td><td>Selecione um dos bancos de dados existentes.</td></tr><tr><td><code>show tables;</code></td><td>Mostrar todas as tabelas disponíveis no banco de dados selecionado.</td></tr><tr><td><code>show columns from &#x3C;table>;</code></td><td>Mostrar todas as colunas no banco de dados selecionado.</td></tr><tr><td><code>select * from &#x3C;table>;</code></td><td>Mostrar tudo na tabela desejada.</td></tr><tr><td><code>select * from &#x3C;table> where &#x3C;column> = "&#x3C;string>";</code></td><td>Procure o necessário <code>string</code>na tabela desejada.</td></tr></tbody></table>
