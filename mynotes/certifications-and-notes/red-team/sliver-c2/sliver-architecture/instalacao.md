> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/sliver-architecture/instalacao.md).

# Instalação

**Instalar o Sliver C2**

* Abra o terminal no Kali Linux.
* Execute:

```
curl https://sliver.sh/install | sudo bash
```

* O comando baixa e executa o script de instalação com privilégios de root.
* Informe a senha do sudo quando solicitado.
* Aguarde a conclusão (pode levar alguns minutos).

**Verificar o status do serviço**

* Após instalar, o Sliver roda como serviço.
* Verifique com:

```
sudo systemctl status sliver
```

* Confirme se aparece: **active (running)**.

**Iniciar o Sliver manualmente**

* O serviço não inicia automaticamente no boot.
* Para iniciar:

```
sudo systemctl start sliver
```

* Depois, acesse o console digitando:

```
sliver
```

**Instalar Mingw64 (opcional, recomendado)**

* Permite gerar payloads para Windows (shellcode, DLL, staged).
* Instale com:

```
sudo apt install mingw-w64
```

* Em muitos casos já está instalado, mas o comando garante as dependências.
