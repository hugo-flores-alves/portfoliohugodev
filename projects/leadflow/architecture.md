# LeadFlow Architecture

## Visão geral

```text
┌─────────────────┐
│ Form / Website  │
└────────┬────────┘
         │ POST JSON
         ▼
┌─────────────────┐
│  n8n Webhook    │
└────────┬────────┘
         ▼
┌─────────────────┐
│ Validate +      │
│ Normalize       │
└────────┬────────┘
         ▼
┌─────────────────┐
│ Lead Scoring    │
└────────┬────────┘
         ▼
┌─────────────────┐
│ Webhook Response│
└─────────────────┘
```

## Decisões técnicas

### Webhook como entrada

Permite conectar o fluxo a praticamente qualquer formulário, site, aplicação ou serviço capaz de realizar uma requisição HTTP.

### Normalização antes da lógica de negócio

Nome, e-mail, tamanho da empresa e interesse são convertidos para um formato previsível antes do scoring. Isso reduz erros nas etapas seguintes.

### Scoring determinístico

O primeiro MVP usa regras explícitas em vez de IA. Isso torna o comportamento reproduzível, barato e fácil de auditar.

### Resposta estruturada

O consumidor recebe o resultado do processamento imediatamente, incluindo `score`, `classification`, dados normalizados e timestamp.

## Evolução para produção

Uma implementação de produção adicionaria autenticação do webhook, persistência, idempotência/deduplicação, observabilidade, error workflow, retry policy e integração com CRM/notificações.
