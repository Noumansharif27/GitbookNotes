> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/dns/question.md).

# Question

1. Encontre todos os registros DNS disponíveis para o domínio "inlanefreight.htb" no servidor de nomes de destino e envie o sinalizador encontrado como um registro DNS como resposta.

* Adicionar `inlanefreight.htb` no arquivo resolvers.txt&#x20;
* Em seguida executar o subbrute com uma lista de nomes para subdominios

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FM2mnTdmvGdmOIeyvwNcS%2Fimage.png?alt=media&amp;token=ad1162fc-5651-4cb0-a321-840b80d4b47f" alt=""><figcaption></figcaption></figure>

* Tentar um zone transfer no subdomio encontrado&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fx2Iv6QFPTga2957DBpez%2Fimage.png?alt=media&amp;token=5d3f2ad5-90d6-43d7-9e1b-bf7e9b4887e5" alt=""><figcaption></figcaption></figure>
