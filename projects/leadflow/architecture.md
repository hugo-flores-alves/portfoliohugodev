# LeadFlow Architecture

## Visão geral

```text
┌──────────────────┐
│ Form / Website   │
└────────┬─────────┘
         │ POST JSON
         ▼
┌──────────────────┐
│ n8n Webhook      │
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Validate +       │
│ Normalize        │
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Lead Scoring     │
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Google Sheets    │
│ Email Lookup     │
└────────┬─────────┘
         ▼
    Duplicate?
      /     \
    yes      no
     │        │
     ▼        ▼
  Stop     Append Row
              │
              ▼
           Hot Lead?
            /    \
          yes     no
           │       │
           ▼       ▼
       Telegram   Finish
```

## Decisões técnicas

### Webhook como entrada

Mantém a automação desacoplada da origem. Qualquer formulário, site ou aplicação capaz de enviar HTTP POST pode alimentar o workflow.

### Normalização antes das regras

Os campos são convertidos para formatos previsíveis antes do scoring e da persistência. Isso reduz inconsistências nas etapas seguintes.

### Scoring determinístico

O score utiliza regras explícitas. Para uma primeira automação comercial isso torna o resultado barato, auditável e fácil de explicar para clientes e recrutadores.

### Deduplicação por e-mail

Antes de persistir um lead, o workflow consulta a coluna `Email` no Google Sheets. Um registro encontrado segue para a resposta de duplicidade; uma busca vazia segue para inserção.

Essa decisão evita duplicação sem exigir banco de dados no MVP.

### Google Sheets como CRM leve

Google Sheets foi escolhido para manter o projeto reproduzível e acessível. Ele funciona como armazenamento simples e permite inspeção visual dos resultados durante demonstrações.

### Telegram para alertas de alta prioridade

Somente leads classificados como `hot` seguem para a notificação. Isso demonstra roteamento condicional e integração orientada a eventos sem gerar alertas desnecessários.

## Segurança

Credenciais Google OAuth2 e Telegram são armazenadas no gerenciador de credenciais do n8n. O workflow versionado utiliza placeholders e não deve conter tokens, Client Secrets, Chat IDs ou URLs privadas.

## Limites do MVP

Google Sheets é adequado para demonstração e volumes pequenos, mas não substitui um banco transacional em cargas maiores. A deduplicação por consulta também pode sofrer condições de corrida em alto volume.

## Caminho para produção

Uma evolução para produção incluiria autenticação/assinatura do webhook, PostgreSQL ou CRM dedicado, constraint única para e-mail, error workflow, retries, observabilidade, rate limiting e gestão de ambientes/segredos.
