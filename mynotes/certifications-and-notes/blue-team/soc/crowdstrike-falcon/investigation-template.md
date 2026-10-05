> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/soc/crowdstrike-falcon/investigation-template.md).

# Investigation Template

#### SOC Investigation Template (EDR / CrowdStrike Falcon)

#### 1. Informações gerais do alerta

* **ID do alerta:**
* **Data/Hora:**
* **Severidade:** (Low / Medium / High / Critical)
* **Status:** (New / In Progress / Closed)
* **Fonte:** (Falcon, SIEM, etc.)

***

#### 2. Contexto do endpoint

* **Hostname:**
* **Tipo:** (Server / Workstation)
* **Sistema operacional:**
* **IP interno/externo:**
* **Localização (se aplicável):**
* **Criticalidade do ativo:** (Baixa / Média / Alta)

***

#### 3. Usuário envolvido

* **Usuário:**
* **Tipo:** (Comum / Admin / Service Account)
* **Privilégios:**
* **Atividade esperada para esse usuário?** (Sim/Não + detalhes)

***

#### 4. Detalhes da detecção

* **Nome da detecção:**
* **Descrição:**
* **MITRE Tactic/Technique:** (ex: Execution / Persistence — MITRE ATT\&CK)
* **Falcon ação:** (Blocked / Detected / Prevented)
* **Processo principal:**
* **Command line:**
* **Hash (SHA256):**

***

#### 5. Análise de processo (Process Tree)

* **Parent process:**
* **Child process(es):**
* **Comportamento suspeito?** (Sim/Não + explicação)

***

#### 6. Análise de rede

* **Conexões externas detectadas?** (Sim/Não)
* **IPs envolvidos:**
* **Domínios/URLs:**
* **País de destino:**
* **Possível C2?** (Sim/Não)

***

#### 7. Threat Intelligence / OSINT

* **Hash verificado?** (Sim/Não)
* **Reputação:** (Malicioso / Suspeito / Limpo)
* **Fontes consultadas:** (ex: VirusTotal, etc.)
* **Relacionamento com grupos conhecidos?**

***

#### 8. Estado atual da ameaça

* **Processo ainda ativo?** (Sim/Não)
* **Persistência identificada?** (Sim/Não)
* **Movimento lateral?** (Sim/Não)
* **Outros endpoints afetados?**

***

#### 9. Ações tomadas

* [ ] Isolamento do host
* [ ] Kill process
* [ ] Quarentena de arquivo
* [ ] Bloqueio de hash/IP/domain
* [ ] Reset de credenciais
* [ ] Nenhuma ação necessária

***

#### 10. Classificação final

* **Tipo:** (True Positive / False Positive / Benign True Positive)
* **Impacto:** (Baixo / Médio / Alto / Crítico)
* **Escopo:** (1 máquina / múltiplas / domínio)

***

#### 11. Conclusão da análise

Resumo claro do que aconteceu:

> (Ex: Execução de PowerShell suspeito via documento Word, sem evidência de persistência. Bloqueado automaticamente pelo Falcon. Sem impacto adicional.)

***

#### 12. Próximos passos / recomendações

* Hardening necessário?
* Ajuste de regra/detecção?
* Treinamento de usuário?
* Monitoramento adicional?

***
