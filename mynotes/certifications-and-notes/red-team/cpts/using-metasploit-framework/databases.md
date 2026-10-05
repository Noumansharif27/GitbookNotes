> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/databases.md).

# Databases

* **Status do PostgreSQL**

```sh
sudo service postgresql status
```

* **Iniciar PostgreSQL**

```sh
sudo systemctl start postgresql
```

* Depois de iniciar o PostgreSQL, precisamos criar e inicializar o banco de dados MSF com `msfdb init`.

```sh
sudo msfdb init
```

* Verificar o status do banco de dados

```sh
sudo msfdb status
```

* **MSF - Conectar ao banco de dados iniciado**

```sh
sudo msfdb run
```

* **MSF - Reiniciar o Banco de Dados**

```sh
msfdb reinit
cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
sudo service postgresql restart
msfconsole -q
```

* **MSF - Opções de banco de dados**

```sh
help database
```

* Criar um workspace

```sh
workspace -a Target_1
```

* Importando resultados de digitalização - nmap

```sh
db_import Target.xml
```

* Usando Nmap dentro do MSFconsole

```sh
db_nmap -sV -sS 10.10.10.8
```

* **MSF - Hosts armazenados**

```sh
hosts -h
```

* **MSF - Serviços Armazenados de Hosts**

```sh
services -h
```

* **MSF - Credenciais Armazenadas**

```sh
creds -h
```

* **MSF - loot Armazenado**

```sh
loot -h
```
