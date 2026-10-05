> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/network-enumeration-with-nmap/saving-the-results.md).

# Saving the Results

`Nmap`podemos salvar os resultados em 3 formatos diferentes.

* Saída normal ( `-oN`) com a `.nmap`extensão do arquivo

* Saída grepável ( `-oG`) com a `.gnmap`extensão do arquivo

* Saída XML ( `-oX`) com a `.xml`extensão do arquivo

* Também podemos especificar a opção ( `-oA`) para salvar os resultados em todos os formatos. O comando poderia ser assim:

```sh
sudo nmap 10.129.2.28 -p- -oA target
```

* **Folhas de estilo**

Com a saída XML, podemos facilmente criar relatórios HTML que são fáceis de ler, mesmo para pessoas não técnicas. Isso é muito útil para documentação, pois apresenta nossos resultados de forma detalhada e clara. Para converter os resultados armazenados do formato XML para HTML, podemos usar a ferramenta `xsltproc`.

```sh
xsltproc target.xml -o target.html
```
