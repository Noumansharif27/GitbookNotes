> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/ipmi.md).

# IPMI

[Intelligent Platform Management Interface](https://www.thomas-krenn.com/en/wiki/IPMI_Basics) ( `IPMI`) é um conjunto de especificações padronizadas para sistemas de gerenciamento de host baseados em hardware usados ​​para gerenciamento e monitoramento de sistemas.

* **Nmap**

```sh
sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
```

* **Metasploit Dumping Hashes**

{% code lineNumbers="true" %}

```
msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes 
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > set rhosts 10.129.42.195
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > show options 
```

{% endcode %}
