> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/footprinting/dns/question.md).

# Question

1. Interaja com o DNS de destino usando seu endereço IP e enumere o FQDN dele para o domínio "inlanefreight.htb".

* `sudo echo '10.129.189.226    inlanefreight.htb' >> /etc/hosts`

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2F4VWft1IdN5aMP3JRaA8O%2Fimage.png?alt=media&amp;token=b4442b58-b7a9-47af-a59f-91bdd20e714e" alt=""><figcaption></figcaption></figure>

2. Identifique se é possível realizar uma transferência de zona e envie o registro TXT como resposta. (Formato: HTB{...))

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2Fh3vCvmO3sJI2tLtwuizp%2Fimage.png?alt=media&amp;token=b9190995-8ee0-4696-b278-9f07b3356426" alt=""><figcaption></figcaption></figure>

3. Qual é o endereço IPv4 do hostname DC1?

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FErOxsYcdDsRlBQQDT87z%2Fimage.png?alt=media&amp;token=489c9d2a-d623-4ec3-8c2f-f42641671161" alt=""><figcaption></figcaption></figure>

4. Qual é o FQDN do host cujo último octeto termina com "xxx203"?

<pre><code>for sub in $(cat /usr/share/wordlists/seclists/Discovery/DNS/fierce-hostlist.txt);do dig $sub.dev.inlanefreight.htb @10.129.189.226  | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
<strong>
</strong><strong>dev1.dev.inlanefreight.htb. 604800 IN	A	10.12.3.6
</strong>ns.dev.inlanefreight.htb. 604800 IN	A	127.0.0.1
win2k.dev.inlanefreight.htb. 604800 IN	A	10.12.3.203
</code></pre>
