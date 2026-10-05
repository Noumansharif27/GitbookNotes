> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/attacking-windows-credential-manager/question.md).

# Question

1. Qual é a senha que mcharles usa para o OneDrive?

* Podemos usar [o comando cmdkey](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cmdkey) para enumerar as credenciais armazenadas no perfil do usuário atual:

```
cmdkey /list
```

* Se nos depararmos com credenciais associadas ao domínio, podemos nos passar por ela executando o `runas`&#x20;

```
runas /savecred /user:SRV01\mcharles cmd
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FqNWdbrPe86YiDTELhRfs%2Fimage.png?alt=media&amp;token=af67059e-c4d1-4dea-b344-cc9ce2623aa1" alt=""><figcaption></figcaption></figure>

* Esse processo de execução abrirá um novo terminal
* Usei a ferramenta [LaZagne](https://github.com/AlessandroZ/LaZagne) para recuperar credenciais armazenadas

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FTwmajSE0AG9u77Q9sSDH%2Fimage.png?alt=media&amp;token=5c2b9ee1-4792-4edd-a7df-24468a4e2151" alt=""><figcaption></figcaption></figure>
