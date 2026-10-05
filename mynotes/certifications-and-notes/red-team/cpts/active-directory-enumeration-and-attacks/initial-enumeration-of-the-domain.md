> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/initial-enumeration-of-the-domain.md).

# Initial Enumeration of the Domain

* Usado para consultar o sistema de nomes de domínio e descobrir o mapeamento do endereço IP para o nome de domínio do destino inserido em um host baseado em Linux.

```sh
nslookup ns1.inlanefreight.com
```

* Usado para iniciar a captura de pacotes de rede na interface de rede, procedendo à `-i`opção de um host baseado em Linux.

```sh
sudo tcpdump -i ens224
```

* Usado para começar a responder e analisar `LLMNR`, `NBT-NS`e `MDNS`consultas na interface especificada, procedendo a `-I`opção e operando no `Passive Analysis`modo que é ativado usando `-A`. Executado a partir de um host baseado em Linux

```sh
sudo responder -I ens224 -A
```

* Executa uma varredura de ping no segmento de rede especificado a partir de um host baseado em Linux.

```sh
fping -asgq 172.16.5.0/23
```

* Executa uma varredura nmap que com detecção de SO, detecção de versão, varredura de script e traceroute habilitados ( `-A`) com base em uma lista de hosts ( `hosts.txt`) especificados no arquivo anterior `-iL`. Em seguida, envia os resultados da varredura para o arquivo especificado após a `-oN`opção. Executado a partir de um host baseado em Linux

```sh
sudo nmap -v -A -iL hosts.txt -oN /home/User/Documents/host-enum
```

#### **Kerbrute**

* Usado `git`para clonar a ferramenta kerbrute de um host baseado em Linux.

```sh
sudo git clone https://github.com/ropnop/kerbrute.git
```

* Usado para listar opções de compilação que são possíveis em `make`um host baseado em Linux.

```sh
make help	
```

* Usado para compilar um `Kerbrute`binário para diversas plataformas de sistemas operacionais e arquiteturas de CPU.

```sh
sudo make all
```

* Usado para testar o `Kebrute`binário compilado escolhido de um host baseado em Linux.

```sh
./kerbrute_linux_amd64
```

* Usado para mover o `Kerbrute`binário para um diretório pode ser definido para estar no caminho de um usuário Linux. Tornando mais fácil usar a ferramenta.

```sh
sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```

* Executa a ferramenta Kerbrute para descobrir nomes de usuários no domínio ( `INLANEFREIGHT.LOCAL`) especificado procedendo a `-d`opção e o controlador de domínio associado especificado procedendo `--dc`usando uma lista de palavras e gera ( `-o`) os resultados para um arquivo especificado. Executado a partir de um host baseado em Linux.

```sh
./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o kerb-results
```
