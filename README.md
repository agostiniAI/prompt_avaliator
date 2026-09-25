# ⚡ Arquitetura Reativa: Webhooks & Avaliador Pré-Prompt

> *"Um webhook é um mecanismo que permite que um serviço externo avise sua aplicação quando um evento ocorre, como um pagamento no Stripe. Ele envia uma requisição HTTP para uma rota da sua aplicação com dados do evento, permitindo atualizar o banco de dados e manter tudo sincronizado em tempo real."*

---

## 🎯 Visão Geral

Este repositório consolida o estudo e a implementação prática de sistemas assíncronos orientados a eventos (*Event-Driven*), superando o modelo ineficiente de consultas periódicas (*Polling*). 

O projeto é composto por duas camadas:
1. **Interface Diagnóstica (Front-End / GitHub Pages):** Uma Single-Page Application (SPA) em HTML5, CSS3 e JavaScript puro que atua como **Avaliador Pré-Prompt**, permitindo auditar a qualidade técnica de instruções antes do envio a modelos de IA.
2. **Serviço Receptor (Back-End / Node.js & Express):** Endpoint HTTP POST estruturado para receber notificações externas em tempo real com confirmação imediata (*Ack 200 OK*), tratamento assíncrono e garantia de idempotência.

---

## 🛑 O Problema: O Gargalo do Polling

No modelo de consulta periódica (*polling*), a aplicação executa requisições repetitivas (ex.: a cada 60 segundos) consultando APIs externas:
> *"Tem novidades? E agora? E agora?"*

### Ineficiências Críticas:
* **Desperdício de Recursos:** Consultar uma API 1.440 vezes ao dia para 3 eventos reais gera mais de 99% de requisições inúteis, esgotando limites de taxa (*rate limits*).
* **Latência Inerente:** Eventos que ocorrem logo após uma consulta aguardam quase o intervalo completo para serem detectados.
* **Sobrecarga de Concorrência:** Chamadas demoradas podem se sobrepor, causando bloqueios e inconsistências no servidor.

---

## ⚡ A Solução: Arquitetura Orientada a Eventos (Webhooks)

Um **Webhook** inverte o fluxo: a aplicação fica em repouso absoluto e o serviço externo (emissor) envia uma notificação no instante exato do acontecimento.
