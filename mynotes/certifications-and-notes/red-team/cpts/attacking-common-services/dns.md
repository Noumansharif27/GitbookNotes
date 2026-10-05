> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/attacking-common-services/dns.md).

# DNS

* **Nmap**&#x20;

```sh
nmap -p53 -Pn -sV -sC 10.10.110.213
```

* **DIG - Transferência de Zona AXFR**

```sh
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```

* Ferramentas como [o Fierce](https://github.com/mschwager/fierce) também podem ser usadas para enumerar todos os servidores DNS do domínio raiz e procurar uma transferência de zona DNS:

```sh
fierce --domain zonetransfer.me
```

* **Enumeração de subdomínio**

```sh
subfinder -d inlanefreight.com -v  
```

* **Subbrute**

{% code lineNumbers="true" %}

```sh
git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
cd subbrute
echo "ns1.inlanefreight.com" > ./resolvers.txt
./subbrute inlanefreight.com -s ./names.txt -r ./resolvers.txt
```

{% endcode %}
