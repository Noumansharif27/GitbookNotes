> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/imap-pop3.md).

# IMAP / POP3

Com a ajuda do `Internet Message Access Protocol`( `IMAP`), o acesso a e-mails de um servidor de e-mail é possível. Ao contrário do `Post Office Protocol`( `POP3`), o IMAP permite o gerenciamento on-line de e-mails diretamente no servidor e suporta estruturas de pastas. Portanto, é um protocolo de rede para o gerenciamento on-line de e-mails em um servidor remoto.

* **Comandos IMAP**

| `1 LOGIN username password`     | Login do usuário.                                                                                               |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `1 LIST "" *`                   | Lista todos os diretórios.                                                                                      |
| `1 CREATE "INBOX"`              | Cria uma caixa de correio com um nome especificado.                                                             |
| `1 DELETE "INBOX"`              | Exclui uma caixa de correio.                                                                                    |
| `1 RENAME "ToRead" "Important"` | Renomeia uma caixa de correio.                                                                                  |
| `1 LSUB "" *`                   | Retorna um subconjunto de nomes do conjunto de nomes que o Usuário declarou como sendo `active`ou `subscribed`. |
| `1 SELECT INBOX`                | Seleciona uma caixa de correio para que as mensagens nela contidas possam ser acessadas.                        |
| `1 UNSELECT INBOX`              | Sai da caixa de correio selecionada.                                                                            |
| `1 FETCH <ID> all`              | Recupera dados associados a uma mensagem na caixa de correio.                                                   |
| `1 CLOSE`                       | Remove todas as mensagens com o `Deleted`sinalizador definido.                                                  |
| `1 LOGOUT`                      | Fecha a conexão com o servidor IMAP.                                                                            |

* **Comandos POP3**

| `USER username` | Identifica o usuário.                                          |
| --------------- | -------------------------------------------------------------- |
| `PASS password` | Autenticação do usuário através de sua senha.                  |
| `STAT`          | Solicita o número de e-mails salvos do servidor.               |
| `LIST`          | Solicita ao servidor o número e o tamanho de todos os e-mails. |
| `RETR id`       | Solicita ao servidor que entregue o e-mail solicitado por ID.  |
| `DELE id`       | Solicita ao servidor que exclua o e-mail solicitado por ID.    |
| `CAPA`          | Solicita ao servidor para exibir os recursos do servidor.      |
| `RSET`          | Solicita ao servidor que redefina as informações transmitidas. |
| `QUIT`          | Fecha a conexão com o servidor POP3.                           |

* ### Footprinting the Service

* Por padrão, portas `110`e `995`são usadas para POP3, e portas `143`e `993`são usadas para IMAP.

* **Nmap**

```sh
sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC
```

* **cURL**

<pre class="language-sh"><code class="lang-sh"><strong>curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd -v
</strong></code></pre>

* **OpenSSL - Interação criptografada TLS POP3**

```sh
openssl s_client -connect 10.129.14.128:pop3s
```

* **OpenSSL - TLS Criptografado Interação IMAP**

```sh
openssl s_client -connect 10.129.14.128:imaps
```
