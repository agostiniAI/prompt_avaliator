# ⚡ Arquitetura Reativa: Webhooks & Termômetro de Automação

> *"Um webhook é um mecanismo que permite que um serviço externo avise sua aplicação quando um evento ocorre, como um pagamento no Stripe. Ele envia uma requisição HTTP para uma rota da sua aplicação com dados do evento, permitindo atualizar o banco de dados e manter tudo sincronizado em tempo real."*

---

## 🎯 Visão Geral do Projeto

Este repositório foi concebido para desmistificar a comunicação assíncrona entre serviços web e erradicar o uso ineficiente de rotinas síncronas de consulta periódica (*polling*). 

O projeto divide-se em duas componentes essenciais:
1. **Interface Diagnóstica (Front-End / GitHub Pages):** Uma Single-Page Application (SPA) desenvolvida em HTML5 semântico, CSS3 moderno e JavaScript puro, que funciona como um **Termômetro de Maturidade**, permitindo auditar a qualidade técnica de integrações e a formulação de prompts[cite: 8].
2. **Microsserviço Receptor (Back-End / Node.js & Express):** Um endpoint HTTP POST estruturado com tratamento assíncrono não-bloqueante (*Event-Loop*), confirmação imediata de recebimento (*Ack 200 OK*) e garantia de resiliência/idempotência.

---

## 🛑 O Problema: O Gargalo do Polling

No modelo tradicional de consulta periódica (*polling*), a aplicação cliente executa scripts repetitivos (por exemplo, a cada 60 segundos) consultando uma API externa:
> *"Há novidades? E agora? E agora?"*

### Principais Ineficiências do Polling:
* **Desperdício Massivo de Recursos:** Consultar uma API 1.440 vezes ao dia para obter apenas 2 ou 3 atualizações reais resulta em mais de 99% de requisições inúteis, esgotando cotas de processamento e atingindo *rate limits*[cite: 4, 10].
* **Latência Inerente:** Se um evento ocorre 2 segundos após uma consulta, o sistema aguardará 58 segundos até a próxima verificação, prejudicando operações críticas[cite: 4, 10].
* **Bloqueios e Concorrência:** Rotinas síncronas ou execuções demoradas podem sobrepor-se, originando colisões de dados e bloqueios desnecessários de infraestrutura.

---

## 🚀 A Solução: Arquitetura Orientada a Eventos (Webhooks)

Um **Webhook** inverte a polaridade da comunicação: a sua aplicação permanece em repouso e o serviço emissor notifica-a no milissegundo exato em que o fato se consuma[cite: 4, 10].
