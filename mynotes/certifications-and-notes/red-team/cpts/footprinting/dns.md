> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/dns.md).

# DNS

DNS é um sistema para resolver nomes de computadores em endereços IP e não tem um banco de dados central.

* **DIG - Consulta NS**

```sh
dig ns <domain.tld> @<nameserver>
```

* **DIG - Consulta de versão**

```sh
dig CH TXT <domain.tld> @<nameserver>
```

* **DIG - QUALQUER Consulta**

```sh
dig any <domain.tld> @<nameserver>
```

* **DIG - Transferência de Zona AXFR**

```sh
dig axfr <domain.tld> @<nameserver>
```

* **DIG - Transferência de Zona AXFR - Interna**

```sh
dig axfr internal.<domain.tld> @<nameserver>
```

* **Força bruta de subdomínio**

```sh
for sub in $(cat /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.<domain.tld> @<nameserver> | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```

* Muitas ferramentas diferentes podem ser usadas para isso, e a maioria delas funciona da mesma forma. Uma dessas ferramentas é, por exemplo, [DNSenum](https://github.com/fwaeytens/dnsenum) .

```sh
dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o subdomains.txt -f /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt <domain.tld>
```
