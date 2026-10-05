> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/shells-and-payloads/web-shells.md).

# Web Shells

Um `web shell`é uma sessão de shell baseada em navegador que podemos usar para interagir com o sistema operacional subjacente de um servidor web.

#### Laudanum

`Laudanum` é um repositório de arquivos prontos que podem ser usados ​​para injetar em uma vítima e receber acesso de volta por meio de um shell reverso, executar comandos no host da vítima diretamente do navegador e muito mais.

* Os arquivos Laudanum podem ser encontrados no

```sh
/usr/share/laudanum
```

* **Mover uma cópia para modificação**

```sh
cp /usr/share/laudanum/aspx/shell.aspx /home/tester/demo.aspx
```

#### Antak Webshell

Antak é um shell da web integrado ao ASP.Net incluído no [projeto Nishang](https://github.com/samratashok/nishang) . Nishang é um conjunto de ferramentas Offensive PowerShell que pode fornecer opções para qualquer parte do seu pentest.&#x20;

* Localização do Antak&#x20;

```sh
 /usr/share/nishang/Antak-WebShell
```

* **Mover uma cópia para modificação**

```sh
cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx
```

* Certifique-se de definir credenciais para acesso ao shell da web.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FYNtyK9GNl6piMa0WDL6i%2Fimage.png?alt=media&amp;token=11af3eca-488c-4b52-8993-c30e3e6f1e9b" alt=""><figcaption></figcaption></figure>
