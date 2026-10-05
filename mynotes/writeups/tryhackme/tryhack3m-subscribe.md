> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/tryhack3m-subscribe.md).

# TryHack3M: Subscribe

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fp1qDHmxQSq5QY9YM6Z6w%2Fimage.png?alt=media&amp;token=ec293ed3-1961-4bd4-b305-0d0bdde1bc98" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="<https://tryhackme.com/r/room/subscribe>" %}

**Classificação** : Medium

**Criadores** :  [ar33zy](https://tryhackme.com/p/ar33zy), [Dex01](https://tryhackme.com/p/Dex01), [1337rce](https://tryhackme.com/p/1337rce)

**Ferramentas** :  rustscan, nmap, gobuster, burpsuite, sqlmap e splunk

### Parte I : Desafio Ofensivo <a href="#part-ii-defensive-challenge-splunk" id="part-ii-defensive-challenge-splunk"></a>

#### Enumeração

```shell
# rustscan -a 10.10.237.52 -- -A
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
😵 https://admin.tryhackme.com

[~] The config file is expected to be at "/root/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.237.52:22
Open 10.10.237.52:80
Open 10.10.237.52:8000
Open 10.10.237.52:8089
Open 10.10.237.52:8191
Open 10.10.237.52:40009
```

Começamos com uma varredura do Nmap e encontramos seis portas abertas, das quais apenas as portas 80 e 40009 são relevantes para a parte ofensiva.

```sh
# nmap -sV -p22,80,8000,8089,8191,40009 -Pn -T5 10.10.237.52 
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-26 23:22 UTC
Nmap scan report for ip-10-10-237-52.eu-west-1.compute.internal (10.10.237.52)
Host is up (0.00033s latency).

PORT      STATE SERVICE         VERSION
22/tcp    open  ssh             OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http            Apache httpd 2.4.41 ((Ubuntu))
8000/tcp  open  http            Splunkd httpd
8089/tcp  open  ssl/http        Splunkd httpd
8191/tcp  open  limnerpressure?
40009/tcp open  http            Apache httpd 2.4.41

```

Visitar a porta 40009 por enquanto nos dá apenas 403 respostas em cada recurso, portanto não podemos enumerá-lo ainda mais.

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FdSm7i6Yq3RL6O2o41Eqb%2FCaptura%20de%20ecra%CC%83%202024-05-26%2C%20a%CC%80s%2020.26.14.png?alt=media&amp;token=ebc8df7a-97ba-4081-bafb-93542f0f1e0b" alt=""><figcaption></figcaption></figure></div>

Passamos para o endpoint na porta 80 e visitamos o site e vemos que há uma página de inscrição e login. O registro está desativado no momento e não conseguimos encontrar nenhum outro diretório à primeira vista via Gobuster

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fdk3w3oyM2i8ONZVvvety%2FCaptura%20de%20ecra%CC%83%202024-05-26%2C%20a%CC%80s%2020.29.07.png?alt=media&amp;token=f9441742-aa92-4ccc-a8e3-26a534e48a75" alt=""><figcaption></figcaption></figure>

Olhando para o código-fonte da página de inscrição, encontramos um pedaço de JavaScript reduzido:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FgCX6aHAeIq9xpBNd7OiY%2FCaptura%20de%20ecra%CC%83%202024-05-26%2C%20a%CC%80s%2020.35.41.png?alt=media&amp;token=571903d5-de21-4218-a03d-53aca4314b89" alt=""><figcaption></figcaption></figure>

Vamos adicionar o domínio `capture3millionsubscribers.thm` no arquivo `/etc/hosts` do nosso kali

```sh
# echo '10.10.237.52 capture3millionsubscribers.thm' >> /etc/hosts                                                             
```

Em seguida alteramos o form action para obter o código de convite `inviteCode1337HM.php`&#x20;

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fjm8XVUjgwHoVqHElDDnJ%2F1.png?alt=media&amp;token=8f83e64d-eb00-44eb-98e6-4845b2fcc5b5" alt=""><figcaption></figcaption></figure></div>

Fornecendo o código de convite, obtemos algumas credenciais.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FSZPq6vMFFSXVDW8uk40W%2F1.png?alt=media&amp;token=7e63d865-dd0b-4f22-b8bf-f5f462a6824f" alt=""><figcaption></figcaption></figure>

#### Exploração

E agora podemos fazer login com as credenciais encontradas

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FMzSMYBDa1YOu687DpKPZ%2F1.png?alt=media&amp;token=cdead205-22f7-42a2-938e-36b6d2adac35" alt=""><figcaption></figcaption></figure>

E fomos redirecionados para um dashboard, enfrentando salas de treinamento divididas em VIP e free.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fc8IyeucjDusxKWLrF50N%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2012.23.25.png?alt=media&amp;token=d53e7b31-f7b2-4736-af91-330892b60e29" alt=""><figcaption></figcaption></figure>

Vemos que o status VIP é definido por um parâmetro Cookie, alteramos para true

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FRc2FSGtKGWcmHsB0oCyZ%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2012.27.23.png?alt=media&amp;token=52eef82d-a42c-4d73-8abb-c5da0812b56d" alt=""><figcaption></figcaption></figure>

Definindo como true, podemos chegar à sala Advanced Red Teaming. Aqui deveremos ser capazes de iniciar uma máquina como estamos acostumados no TryHackMe.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FNV9LT1tkT3t6ot69Nntd%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2012.29.00.png?alt=media&amp;token=b0182d8c-7406-4e52-bd10-e0563670bb12" alt=""><figcaption></figcaption></figure>

Verificando a fonte, vemos um script referenciando um campo oculto para verificar o status VIP

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FhejJos5TF00LtaEewD8L%2F1.png?alt=media&amp;token=abb0ea9d-ed47-415c-aecf-e3e7e2757e20" alt=""><figcaption></figcaption></figure>

Ele faz uma solicitação para`/BBF813FA941496FCE961EBA46D754FF3.php`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsGNcwvwdvCtd7qlp66sr%2F1.png?alt=media&amp;token=e5edf0f3-3ffe-48e3-9b26-50ca8edb190c" alt=""><figcaption></figcaption></figure>

Depois de adicionar `admin1337special.hackme.thm`em `/etc/host`entrar em contato com ele, somos redirecionados para `/public/html`, mas este site ainda nos dá um `403`.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FGeRsnngOn4cuqGjiQdhg%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.01.09.png?alt=media&amp;token=23fde208-fc0e-4778-b8b2-e44f4a67b15d" alt=""><figcaption></figcaption></figure>

Usando o gobuster nesse diretório conseguimos localizar a página de login, talvez aqui possamos fazer uso do token.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FPlGH1FAKlD8AauKYCJNh%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.13.46.png?alt=media&amp;token=bb3651f6-5e3a-4e06-93bc-4866b8e6c81c" alt=""><figcaption></figcaption></figure>

E temos uma página para fornecer um token. Usamos o token.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fzy8KGVi0S4GtT7jV0v7m%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.15.54.png?alt=media&amp;token=6b282cdd-b018-4c41-bf1f-dff82fb3386a" alt=""><figcaption></figcaption></figure>

Depois de fornecer o token, somos solicitados a fazer login, mas não encontramos nenhum crédito de administrador até agora. Tentar os créditos de inscrição não funciona.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZdTq5lTAyYNVtoRSjVua%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.18.37.png?alt=media&amp;token=9aaa2135-26c3-4f36-95c1-21ce27ac4e58" alt=""><figcaption></figcaption></figure>

Capturamos uma solicitação de login arbitrária para usar o SQLMap, talvez consigamos despejar o banco de dados atrás do login.

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZGkBShQf7q9UZgW8g7nL%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.26.15.png?alt=media&amp;token=66abed32-a858-48a7-b666-4a1b198b19c1" alt=""><figcaption></figcaption></figure></div>

Salvar em um arquivo txt

{% code lineNumbers="true" %}

```sh
$ cat myinj 

POST /api/login.php HTTP/1.1
Host: admin1337special.hackme.thm:40009
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://admin1337special.hackme.thm:40009/public/html/login
Content-Type: application/json
Origin: http://admin1337special.hackme.thm:40009
Content-Length: 39
Connection: close
Cookie: PHPSESSID=vdcfcvlf88u99gjocfefqj1lpr

{"username":"admin","password":"12345"}    
```

{% endcode %}

Acionando o SLQMap na solicitação, podemos fazer dump do banco de dados

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FxjsifuCKWx7LjzC7StEv%2F1.png?alt=media&amp;token=7f3153b0-d9cf-46bd-a416-7659eeb1549d" alt=""><figcaption></figcaption></figure>

Após o login, estamos no painel e podemos selecionar uma ação, escolhendo Inscrever-se e depois Definir Opções, nada acontece à primeira vista.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F3XHk7pHl8b2FklnkvRIB%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2013.53.08.png?alt=media&amp;token=42ef1e0c-f36f-4da7-8a80-e389cfa80ed6" alt=""><figcaption></figcaption></figure>

Mas depois de voltar para hackme.thm/ a bandeira final é apresentada.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FttpsPQYhTjeP7OBEGw0T%2F1.png?alt=media&amp;token=f2104c03-fd06-4b8b-ae62-9115a5815cc2" alt=""><figcaption></figcaption></figure>

### Parte II : Desafio Defensivo <a href="#part-ii-defensive-challenge-splunk" id="part-ii-defensive-challenge-splunk"></a>

Vamos para Pesquisa e relatórios e fornecemos a seguinte consulta.

`index=*`com um tempo de `All time`.&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FvymUpdegyF8LRDjK31za%2F1.png?alt=media&amp;token=069a8009-d838-42b2-8503-314dc6974bea" alt=""><figcaption></figcaption></figure>

Observamos o user-agent para descobrir quais requisições foram feitas, e descobrimos a ferramenta

<div align="left"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FDE9mXa9X2lisLXkTJoAr%2F1.png?alt=media&amp;token=d305adbe-423c-46e4-8c64-0c9945fea17d" alt="" width="452"><figcaption></figcaption></figure></div>

Para descobrir o IP de origem do invasor basta filtrar pelo user\_agent.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fu49ZHY4E1hH2mdwKkBob%2F1.png?alt=media&amp;token=f178bbdd-3be9-414c-8e51-0451ecaa7f25" alt=""><figcaption></figcaption></figure>

Para filtrar os eventos observados pelo ip filtramos pelo ip:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZVlbrjRt7YNw5Ip5F6Yz%2F1.png?alt=media&amp;token=4c38cf47-0ec9-4b40-a528-c78d22293abf" alt=""><figcaption></figcaption></figure>

Tentamos filtrar todas as consultas feitas no bloco de dados bruto para determinar a tabela de destino.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FF33PddlxpNkmeeXO1pSI%2F1.png?alt=media&amp;token=9111ec5f-08e2-4576-844b-515fcda5e97e" alt=""><figcaption></figcaption></figure>

Usamos o [cyberchef](https://gchq.github.io/CyberChef/) para decodificar o URI

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F2MCGlgvK9LoMsGK5jYhE%2F1.png?alt=media&amp;token=d3163d28-9565-4526-8614-a0482078f448" alt=""><figcaption></figcaption></figure>
