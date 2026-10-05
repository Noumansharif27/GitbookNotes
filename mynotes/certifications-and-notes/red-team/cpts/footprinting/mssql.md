> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/mssql.md).

# MSSQL

[O Microsoft SQL](https://www.microsoft.com/en-us/sql-server/sql-server-2019) ( `MSSQL`) é o sistema de gerenciamento de banco de dados relacional baseado em SQL da Microsoft.

* **Verificação de script NMAP MSSQL**

```sh
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
```

* **Ping MSSQL no Metasploit**
* **Payload** :  `auxiliary/scanner/mssql/mssql_ping`
* **Conectando com Mssqlclient.py**

```sh
python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth
```
