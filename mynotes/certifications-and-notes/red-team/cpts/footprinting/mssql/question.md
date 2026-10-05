> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/mssql/question.md).

# Question

1. Enumere o alvo usando os conceitos ensinados nesta seção. Liste o nome do host do servidor MSSQL.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FfUmNY0wONatJg0RDhixU%2Fimage.png?alt=media&amp;token=4530bf7b-6241-47f8-84d2-c44e84f9375c" alt=""><figcaption></figcaption></figure>

2. Conecte-se à instância do MSSQL em execução no destino usando a conta (backdoor:Password1) e liste o banco de dados não padrão presente no servidor.

* Conetar-se a instância do MSSQL

```sh
python3 mssqlclient.py backdoor@10.129.13.142 -windows-auth
```

* Payload&#x20;

```sql
select name from sys.databases
```
