> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/sliver-c2/sliver-architecture/protocolos.md).

# Protocolos

**HTTP**

* Comunicação em texto claro (não criptografada).
* Fácil de implementar e amplamente permitido em redes.
* Mais suscetível a detecção por ferramentas de segurança.
* Usado em cenários onde simplicidade é suficiente ou para testes básicos.

**HTTPS**

* Versão segura do HTTP com criptografia TLS.
* Dificulta a inspeção do tráfego por ferramentas defensivas.
* Muito comum em redes corporativas → ajuda na camuflagem.
* Ideal para operações mais furtivas.

**mTLS (Mutual TLS)**

* Autenticação bidirecional (cliente e servidor se validam).
* Aumenta significativamente a segurança da comunicação.
* Evita conexões não autorizadas ao C2.
* Útil em ambientes onde controle de acesso é crítico.

**DNS**

* Comunicação via consultas DNS.
* Alta evasão, pois DNS geralmente é permitido em redes.
* Baixa largura de banda → comunicação mais lenta.
* Ideal para ambientes altamente restritivos.

**WireGuard**

* Protocolo VPN moderno e leve.
* Criptografia forte e alto desempenho.
* Comunicação mais estável e difícil de interceptar.
* Pode ser menos “discreto” dependendo da rede (pode chamar atenção).
