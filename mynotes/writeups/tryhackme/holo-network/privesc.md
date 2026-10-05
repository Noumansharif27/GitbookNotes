> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/writeups/tryhackme/holo-network/privesc.md).

# PrivEsc

#### Task 20

Ao executar o comando `find / -perm -u=s -type f 2>/dev/null` descobrimmos o binário SUID, encontramos algo muito interessante. binário `docker` tem o bit SUID definido! Isso significa que podemos usar esse binário para escalonamento de privilégios.

<figure><img src="https://4024756925-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FZbLrq3t9Su3CqGmkXz7o%2Fuploads%2FyHPC7anfbeFlQ4zCCZY3%2Fimage.png?alt=media&amp;token=f0914471-80fc-48af-b196-8fdb89293d11" alt=""><figcaption></figcaption></figure>
