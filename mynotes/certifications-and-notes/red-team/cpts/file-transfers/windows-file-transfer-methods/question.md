> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/windows-file-transfer-methods/question.md).

# Question

1. Baixe o arquivo flag.txt da raiz da web usando wget do Pwnbox. Envie o conteúdo do arquivo como sua resposta.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FR1PQUgR3fJ5SurWJf0X8%2Fimage.png?alt=media&amp;token=6fc09bb2-8f41-4289-a301-28f97f5cc2a9" alt=""><figcaption></figcaption></figure>

2. Carregue o arquivo anexado chamado upload\_win.zip para o alvo usando o método de sua escolha. Depois de carregado, descompacte o arquivo e execute "hasher upload\_win.txt" na linha de comando. Envie o hash gerado como sua resposta.

* Iniciar um servidor `ftp`  no linux&#x20;

```sh
sudo python3 -m pyftpdlib --port 21
```

* Fazer o Download do arquivo no windows&#x20;

```powershell
 (New-Object Net.WebClient).DownloadFile('ftp://10.10.15.221/upload_win.txt', 'C:\Users\Public\upload_win.txt')
```

* Utilizar a ferramenta `hasher` para gerar o hash&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FBAkXOGCdt5fewA7ZNC6D%2Fimage.png?alt=media&amp;token=5a9eb599-b528-4059-9d1e-ca0d67468178" alt=""><figcaption></figcaption></figure>
