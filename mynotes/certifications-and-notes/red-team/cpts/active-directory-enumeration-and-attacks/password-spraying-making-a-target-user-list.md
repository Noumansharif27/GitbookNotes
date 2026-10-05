> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/password-spraying-making-a-target-user-list.md).

# Password Spraying - Making a Target User List

* **Usando enum4linux**

```sh
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

* **Usando rpcclient**

```sh
rpcclient -U "" -N 172.16.5.5
```

`enumdomusers`

* **Usando o sinalizador CrackMapExec --users**

```sh
crackmapexec smb 172.16.5.5 --users
```

* **Usando ldapsearch**

```sh
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

* **Usando windapsearch**

```sh
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```

* **Enumeração de usuário Kerbrute**

```sh
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt 
```

* **Usando CrackMapExec com credenciais válidas**

```sh
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```
