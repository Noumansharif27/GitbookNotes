> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/password-mutations-and-custom-wordlists.md).

# Password Mutations & Custom Wordlists

* Utiliza o cewl para gerar uma lista de palavras com base nas palavras-chave presentes em um site.

```
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

* Utiliza o Hashcat para gerar uma lista de palavras baseada em regras

```
hashcat --force password.list -r custom.rule --stdout > mut_password.list
```

* &#x20;Gerar uma lista de possíveis nomes de usuário com a ferramenta `username-anarchy`&#x20;

```
./username-anarchy -i /path/to/listoffirstandlastnames.txt
```

* Baixar uma lista de extensões&#x20;

```
curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```
