> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/credential-hunting-in-linux.md).

# Credential Hunting in Linux

* Procurando arquivos de configuração

```bash
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```

* Pesquisando bancos de dados

```bash
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```

* Procurando por anotações

```bash
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```

* Procurando por scripts

```bash
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```

* Enumerando tarefas cron

```bash
cat /etc/crontab 
```

* Enumerando arquivos de histórico

```bash
tail -n5 /home/*/.bash*
```

* Enumerando arquivos de log

```bash
for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```

* **Memory and cache - Mimipenguin**

```bash
sudo python3 mimipenguin.py
```

* **LaZagne**

```bash
sudo python2.7 laZagne.py all
```

* **Browser credentials**

{% code lineNumbers="true" %}

```bash
ls -l .mozilla/firefox/ | grep default 
cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```

{% endcode %}

* A ferramenta [Firefox Decrypt](https://github.com/unode/firefox_decrypt) é excelente para descriptografar essas credenciais e é atualizada regularmente.

```bash
python3.9 firefox_decrypt.py
```

* Alternativamente, `LaZagne`também pode retornar resultados se o usuário tiver utilizado um navegador compatível.

```bash
python3 laZagne.py browsers
```
