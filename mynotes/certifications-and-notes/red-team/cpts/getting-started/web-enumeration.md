> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/web-enumeration.md).

# Web Enumeration

* Execute uma verificação de diretório em um site

```sh
gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt
```

* Execute uma verificação de subdomínio em um site

```sh
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

* Pegue o banner do site

```sh
curl -IL https://www.inlanefreight.com
```

* Listar detalhes sobre o servidor web/certificados

```sh
whatweb 10.10.10.121
```

* Listar diretórios potenciais em`robots.txt`

```sh
curl 10.10.10.121/robots.txt
```

* Ver fonte da página (no Firefox)

```sh
ctrl+U
```
