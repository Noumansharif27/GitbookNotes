> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/privilege-escalation.md).

# Privilege Escalation

* Execute `linpeas`o script para enumerar o servidor remoto

```sh
./linpeas.sh
```

* Listar `sudo`privilégios disponíveis

```sh
sudo -l
```

* Execute um comando com`sudo`

```sh
sudo -u user /bin/echo Hello World!
```

* Mudar para usuário root (se tivermos acesso a `sudo su`)

```sh
sudo su -
```

* Mudar para um usuário (se tivermos acesso a ele `sudo su`)

```sh
sudo su user -
```

* Crie uma nova chave SSH

```sh
ssh-keygen -f key
```

* Adicione a chave pública gerada ao usuário

```sh
echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

* SSH para o servidor com a chave privada gerada

```sh
ssh root@10.10.10.10 -i key
```
