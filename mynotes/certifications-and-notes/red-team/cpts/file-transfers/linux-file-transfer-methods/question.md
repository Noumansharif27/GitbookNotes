> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/file-transfers/linux-file-transfer-methods/question.md).

# Question

1. Baixe o arquivo flag.txt da raiz da web usando Python do Pwnbox. Envie o conteúdo do arquivo como sua resposta.

* **Script python para dowload de arquivos**

{% code lineNumbers="true" %}

```python
import argparse
import requests

def download_file(url, output_file):
    try:
        # Enviar uma solicitação GET para a URL
        response = requests.get(url)
        
        # Verificar se a solicitação foi bem-sucedida (código de status 200)
        if response.status_code == 200:
            # Escrever o conteúdo no arquivo especificado
            with open(output_file, 'w') as file:
                file.write(response.text)
            print(f"Arquivo salvo como {output_file}")
        else:
            print(f"Erro ao baixar o arquivo. Código de status: {response.status_code}")
    except Exception as e:
        print(f"Uma exceção ocorreu: {e}")

if __name__ == "__main__":
    # Configurar o parser de argumentos
    parser = argparse.ArgumentParser(description='Baixe um arquivo da web e salve-o localmente.')
    parser.add_argument('url', type=str, help='URL do arquivo a ser baixado.')
    parser.add_argument('output_file', type=str, help='Nome do arquivo onde o conteúdo será salvo.')
    
    # Analisar os argumentos da linha de comando
    args = parser.parse_args()
    
    # Chamar a função para baixar o arquivo
    download_file(args.url, args.output_file
```

{% endcode %}

* Executar o script&#x20;

```sh
python download_file.py http://example.com/flag.txt flag.txt
```

1. Carregue o arquivo anexado chamado upload\_nix.zip para o alvo usando o método de sua escolha. Uma vez carregado, SSH para a caixa, extraia o arquivo e execute "hasher \<arquivo extraído>" na linha de comando. Envie o hash gerado como sua resposta.

* Carregar o arquivo com o `scp`&#x20;

```sh
scp upload_nix.txt  htb-student@10.129.218.102:/home/htb-student
```

* Extrair o hash com o hasher

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FKy2ncIdzetEBgNFqJx3o%2Fimage.png?alt=media&amp;token=a1c37e4a-8e59-4072-89b0-1ec2696ae9dc" alt=""><figcaption></figcaption></figure>
