> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/getting-started/web-enumeration/questoes.md).

# Questões

1. Tente executar algumas das técnicas de enumeração da web que você aprendeu nesta seção no servidor acima e use as informações obtidas para obter o sinalizador.

* Uma solicitação com o `cURL` em robots.txt

```
curl http://94.237.51.8:33847/robots.txt; echo
User-agent: *
Disallow: /admin-login-page.php
```

* Observamos que existe um diretório, vamos ver o código fonte

```
$ curl -i http://94.237.51.8:33847/admin-login-page.php; echo 
HTTP/1.1 200 OK
Date: Sun, 14 Jul 2024 20:17:14 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: PHPSESSID=ekt18e2dgqedkr86cjjkcsu019; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Vary: Accept-Encoding
Content-Length: 1666
Content-Type: text/html; charset=UTF-8

...SNIP...

<!-- TODO: remove test credentials admin:password123 -->

```

Encontramos as credenciais de login, vamos tentar fazer com essas credenciais

```
$ curl -X POST -d 'username=admin&password=password123' http://94.237.51.8:33847/admin-login-page.php | grep "HTB{"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1368  100  1333  100    35   5174    135 --:--:-- --:--:-- --:--:--  5322
            <center><strong>HTB{w3b_3num3r4710n_r3v34l5_53cr375}</strong></center>
```
