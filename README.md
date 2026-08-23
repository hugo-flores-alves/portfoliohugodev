# FlowForge Automations

Portfólio de automações e integrações construídas com **n8n**, APIs, webhooks e IA.

Projetos orientados a problemas reais de negócio, com foco tanto em **vagas de automação/integration engineering** quanto em **projetos freelance**.

## Projetos

| Projeto | Problema resolvido | Tecnologias | Status |
|---|---|---|---|
| [Prospectador AI](./projects/prospectador-ai/) | Descoberta, qualificação por IA e priorização de empresas para prospecção B2B | n8n, Apify, OpenAI, Google Sheets, JavaScript | 🚀 MVP funcional |
| [LeadFlow](./projects/leadflow/) | Qualificação, deduplicação, persistência e alerta de leads | n8n, Google Sheets, Telegram, Webhooks, JavaScript | ✅ Funcional |
| [SmartCRM](./projects/smartcrm/) | Pipeline comercial, atualização de leads e follow-ups automáticos | n8n, Google Sheets, Telegram, Schedule Trigger, JavaScript | ✅ Funcional |
| [SupportFlow](./projects/supportflow/) | Triagem de suporte com regras e classificação por IA | n8n, Google Sheets, Telegram, OpenRouter, LLM, JavaScript | 🤖 AI V2 |

## Projeto em destaque — Prospectador AI

```text
Google Maps / Apify
        ↓
Normalização + ICP
        ↓
Pré-qualificação
        ↓
OpenAI
        ↓
ICP Score + Opportunity Score
        ↓
Tier A+ / A / B / C / D
        ↓
CRM Google Sheets
        ↓
Fila comercial + aprovação humana
```

Sistema de prospecção B2B que transforma buscas por nicho e localização em uma fila comercial priorizada. A coleta é feita via Apify, o n8n normaliza e filtra os dados, e a OpenAI analisa os leads usando critérios de ICP, oportunidade e evidências disponíveis.

A solução foi validada ponta a ponta e já grava os resultados em um CRM estruturado no Google Sheets. A próxima fase é medir a conversão da fila aprovada em conversas, reuniões e clientes.

[Ver Prospectador AI](./projects/prospectador-ai/)

## Outros projetos funcionais

### LeadFlow

`Webhook → Validação → Lead Scoring → Deduplicação → Google Sheets → HOT? → Telegram`

Recebe leads via HTTP, normaliza e valida os dados, calcula prioridade comercial, evita duplicados, persiste novos registros e alerta oportunidades quentes.

### SmartCRM

`Atualização comercial → Pipeline + Schedule → Follow-up → Telegram`

Organiza estágio, último contato, próximo follow-up e observações. Um segundo workflow monitora pendências e envia alertas.

### SupportFlow

`Ticket → Validação → IA → Google Sheets → Crítico? → Telegram`

Automatiza a triagem de atendimento usando regras e classificação semântica por IA.

## O que este portfólio demonstra

- Automação de processos de negócio com n8n
- Integração via REST APIs e webhooks
- Coleta de dados comerciais com Apify
- Integração com OpenAI e outros LLMs
- Prompting com respostas estruturadas em JSON
- Scoring, ICP e priorização comercial
- Google Sheets como camada de CRM
- Tratamento, normalização e deduplicação de dados
- Roteamento condicional e workflows agendados
- Aprovação humana em processos de IA
- Gerenciamento seguro de credenciais
- Documentação técnica e workflows reproduzíveis

## Segurança

**Nenhuma credencial, token, senha, Client Secret ou API key é versionada neste repositório.** As integrações públicas usam placeholders; segredos ficam no gerenciador de credenciais do n8n ou em variáveis de ambiente.

## Sobre

**FlowForge Automations** é um portfólio focado em automação, integrações e soluções low-code/no-code com extensão por código e IA.

O objetivo não é apenas mostrar workflows, mas demonstrar sistemas que atacam problemas reais de aquisição, vendas, CRM e suporte.
