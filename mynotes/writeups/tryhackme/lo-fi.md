> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/lo-fi.md).

# Lo-Fi

<div data-full-width="false"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FC2TWfuVZvbHDpgqskUdx%2Fimage.png?alt=media&amp;token=a81cc5c9-b7a2-43e7-96e4-58afb81d9403" alt="" width="450"><figcaption></figcaption></figure></div>

**Criador :** [cmnatic](https://tryhackme.com/p/cmnatic)

**Classificação :** Fácil

**Ferramentas:** nmap, curl

#### Enumeração

Varredura de porta com a ferramenta `nmap`, `curl`

```shell
sudo nmap -sC -sV 10.10.15.183 
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-22 12:57 EDT
Nmap scan report for 10.10.15.183
Host is up (0.21s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 8a:60:56:8e:64:ee:7c:f1:72:5d:a6:99:ef:84:3f:b9 (RSA)
|   256 62:d8:72:78:0b:bb:af:68:19:6a:7e:c4:ab:7a:87:f3 (ECDSA)
|_  256 fa:55:bb:e7:f7:95:ef:7c:9a:1d:d9:a0:3d:96:99:b2 (ED25519)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-title: Lo-Fi Music
|_http-server-header: Apache/2.2.22 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.31 seconds

```

Existe uma porta `80-http` isso significa que possui uma página web

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F1z528k3UuuOa3vCdGV0z%2Fimage.png?alt=media&amp;token=8eb3660f-d861-409b-9fdf-f0d108fa9905" alt=""><figcaption></figcaption></figure>

Clicando em um dos campos, somos rediriciados a uma outra página, e notas que o paramêtro `page=` é vulnerável a `LFI`&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FbHkhgUcxfSTONCPylDuW%2Fimage.png?alt=media&amp;token=6977aae0-9242-448f-ac3a-448fe4bb9703" alt=""><figcaption></figcaption></figure>

#### Exploração

De acordo com a sala `THM`, teremos que encontrar a flag na raiz do sistema, usei logo `curl` filtrando a flag

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FfzTtAeCTUb8StXubI7kt%2Fimage.png?alt=media&amp;token=b38cb74f-056e-4855-92f1-e6c63aaecc57" alt=""><figcaption></figcaption></figure>
