> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/infrastructure-based-enumeration.md).

# Infrastructure-based Enumeration

* Transparência de certificado.

```
curl -s https://crt.sh/\?q\=<target-domain>\&output\=json | jq .
```

* Podemos filtrar por subdomínios únicos

```
curl -s https://crt.sh/\?q\=<target-domain>\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

* Servidores hospedados apenas pela empresa

```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep target.com | cut -d" " -f1,4;done
```

* Escaneie cada endereço IP em uma lista usando o Shodan.

{% code lineNumbers="true" %}

```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep target.com| cut -d" " -f4 >> ip-addresses.txt;done
for i in $(cat ip-addresses.txt);do shodan host $i;done
```

{% endcode %}

* Registros DNS

```
dig any target.com
```
