> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/enumerating-and-retrieving-password-policies.md).

# Enumerating & Retrieving Password Policies

* **Enumerando a política de senha - do Linux - Credentialed**

```sh
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```

* **Usando rpcclient**

```sh
rpcclient -U "" -N 172.16.5.5
```

`querydominfo`

`getdompwinfo` : Obtendo a politica de senha&#x20;

* **Usando enum4linux**

```
enum4linux -P 172.16.5.5
```

* **Usando enum4linux-ng**

```sh
enum4linux-ng -P 172.16.5.5 -oA ilfreight
```

* **Exibindo o conteúdo de ilfreight.json**

```sh
cat ilfreight.json
```

* **Usando ldapsearch**

```sh
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

#### Enumerando a política de senha - do Windows

* **Usando net.exe**

```powershell
net accounts
```

* **Usando o PowerView**

{% code lineNumbers="true" %}

```powershell
import-module .\PowerView.ps1
Get-DomainPolicy
```

{% endcode %}
