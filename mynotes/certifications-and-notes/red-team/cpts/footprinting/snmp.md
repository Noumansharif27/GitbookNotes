> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/snmp.md).

# SNMP

`Simple Network Management Protocol`( [SNMP](https://datatracker.ietf.org/doc/html/rfc1157) ) foi criado para monitorar dispositivos de rede. Além disso, esse protocolo também pode ser usado para lidar com tarefas de configuração e alterar configurações remotamente.&#x20;

**Footprinting the Service**

* Para footprinting SNMP, podemos usar ferramentas como `snmpwalk`, `onesixtyone`, e `braa`. `Snmpwalk`é usado para consultar os OIDs com suas informações. `Onesixtyone`pode ser usado para forçar brutamente os nomes das strings da comunidade, já que elas podem ser nomeadas arbitrariamente pelo administrador. Já que essas strings da comunidade podem ser vinculadas a qualquer fonte, identificar as strings da comunidade existentes pode levar algum tempo.

* **SNMPwalk**

```sh
snmpwalk -v2c -c public <FQDN/IP>
```

* **OneSixtyOne**

{% code lineNumbers="true" %}

```sh
sudo apt install onesixtyone
onesixtyone -c /usr/share/wordlists/seclists/Discovery/SNMP/snmp.txt <FQDN/IP>
```

{% endcode %}

* **Braa**

{% code lineNumbers="true" %}

```sh
sudo apt install braa
braa <community string>@<IP>:.1.3.6.*   # Syntax
braa public@10.129.14.128:.1.3.6.*
```

{% endcode %}
