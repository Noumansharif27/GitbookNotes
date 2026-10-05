> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/mysql/question.md).

# Question

1. Enumere o servidor MySQL e determine a versão em uso. (Formato: MySQL XXXX)

* **Login** : `mysql -u robin -probin -h 10.129.8.243`
* **Comando para a versão** :  `select version();`

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fy86qIA6R9J12tyZeGUaf%2Fimage.png?alt=media&amp;token=841190e5-8555-4189-9da9-9d632a8f8da1" alt=""><figcaption></figcaption></figure></div>

2. Durante nosso teste de penetração, encontramos credenciais fracas "robin:robin". Devemos tentar isso no servidor MySQL. Qual é o endereço de e-mail do cliente "Otto Lang"?

* **Listar as base de dados** :`show tables;`

* **Usar a base de dados** : `USE customers;`

* **Mostrar as tabelas** : `show tables;`

* Listar resultados com nomes especificos&#x20;

```sql
select * from myTable WHERE name LIKE 'Otto Lang%';
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FwDVeF7AQJ4GFu93jV0C9%2Fimage.png?alt=media&amp;token=6e3b7c12-6f00-4d7c-b784-c072ad79116d" alt=""><figcaption></figcaption></figure>
