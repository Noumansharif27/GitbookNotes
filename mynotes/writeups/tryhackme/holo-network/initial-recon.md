> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/holo-network/initial-recon.md).

# Initial Recon

**Escopo** : 10.200.x.0/24 e 192.168.100.0/24

```sh
$ nmap -sV -sC -p- 10.200.95.0/24 

Starting Nmap 7.93 ( https://nmap.org ) at 2024-06-02 22:57 UTC
Nmap scan report for ip-10-200-95-33.eu-west-1.compute.internal (10.200.95.33)
Host is up (0.010s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f689b641a8505a97a467cef20a85e29e (RSA)
|   256 47f5e5c9d6a0defed325e3ccda297a45 (ECDSA)
|_  256 2a773021585c1f5d3d2f2c8f8c3f492a (ED25519)
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-generator: WordPress 5.5.3
| http-robots.txt: 21 disallowed entries (15 shown)
| /var/www/wordpress/index.php 
| /var/www/wordpress/readme.html /var/www/wordpress/wp-activate.php 
| /var/www/wordpress/wp-blog-header.php /var/www/wordpress/wp-config.php 
| /var/www/wordpress/wp-content /var/www/wordpress/wp-includes 
| /var/www/wordpress/wp-load.php /var/www/wordpress/wp-mail.php 
| /var/www/wordpress/wp-signup.php /var/www/wordpress/xmlrpc.php 
| /var/www/wordpress/license.txt /var/www/wordpress/upgrade 
|_/var/www/wordpress/wp-admin /var/www/wordpress/wp-comments-post.php
|_http-title: holo.live
33060/tcp open  mysqlx?
| fingerprint-strings: 
|   DNSStatusRequestTCP, LDAPSearchReq, NotesRPC, SSLSessionReq, TLSSessionReq, X11Probe, afp: 
|     Invalid message"
|_    HY000

                                ...SNIP...

Nmap scan report for ip-10-200-95-250.eu-west-1.compute.internal (10.200.95.250)
Host is up (0.0078s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d8217f15756577cd6b47b6a9ff4eefb1 (RSA)
|   256 e66a175cdf57b6f4ed07b0c3a4bf60a5 (ECDSA)
|_  256 df424a4d7804abf4e8a905242d030ae3 (ED25519)
1337/tcp open  http    Node.js Express framework
|_http-title: Error
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (2 hosts up) scanned in 46.42 seconds
```

Qual é o último octeto do endereço IP do servidor web público?

`33`

Quantas portas estão abertas no servidor web?

`3`

Qual CME está sendo executado na porta 80 do servidor web?

`wordpress`

Qual versão do CME está sendo executada na porta 80 do servidor web?

`5.5.3`

Qual é o título HTTP do servidor web?

```sh
# curl -I 10.200.95.33
HTTP/1.1 200 OK
Date: Sun, 02 Jun 2024 23:08:37 GMT
Server: Apache/2.4.29 (Ubuntu)
X-UA-Compatible: IE=edge
Link: <http://www.holo.live/index.php/wp-json/>; rel="https://api.w.org/"
Content-Type: text/html; charset=UTF-8

```

`holo.live`&#x20;
