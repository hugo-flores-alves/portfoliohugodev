# FlowForge Automations

Portfolio de automações e integrações construídas com **n8n**, APIs, webhooks e IA.

Projetos orientados a problemas reais de negócio, com foco tanto em **vagas de automação/integration engineering** quanto em **projetos freelance**.

## Projetos

| Projeto | Problema resolvido | Tecnologias | Status |
|---|---|---|---|
| [LeadFlow](./projects/leadflow/) | Qualificação, deduplicação, persistência e alerta de leads | n8n, Google Sheets, Telegram, Webhooks, JavaScript | ✅ Funcional |
| SmartCRM | Organização de pipeline e follow-up | n8n, CRM, automações | 🚧 Próximo |
| SupportFlow | Atendimento e triagem automatizada | n8n, Telegram, APIs | 🚧 Planejado |
| DataSync | Sincronização e transformação entre APIs | n8n, REST APIs, ETL | 🚧 Planejado |
| AI Assistant | Automação com IA e recuperação de contexto | n8n, LLM, RAG | 🚧 Planejado |

## Projeto em destaque — LeadFlow

O LeadFlow é uma automação comercial executável que:

```text
Webhook → Validação → Lead Scoring → Deduplicação → Google Sheets → HOT? → Telegram
```

Ele recebe leads via HTTP, normaliza e valida os dados, calcula prioridade comercial, evita e-mails duplicados, persiste novos registros e alerta imediatamente quando uma oportunidade recebe classificação `hot`.

[Ver documentação do LeadFlow](./projects/leadflow/)

## O que este portfólio demonstra

- Automação de processos de negócio
- Integração via REST APIs e webhooks
- Google OAuth 2.0 e APIs Google
- Integração com Telegram Bot API
- Tratamento e transformação de dados
- Validação e regras de negócio
- Deduplicação e roteamento condicional
- Gerenciamento seguro de credenciais
- Documentação técnica e workflows reproduzíveis

## Estrutura

```text
portfoliohugodev/
├── README.md
└── projects/
    └── leadflow/
        ├── README.md
        ├── architecture.md
        ├── workflow.json
        ├── workflow-v2.json
        └── sample-payload.json
```

## Sobre

**FlowForge Automations** é uma identidade de portfólio focada em automação, integrações e soluções low-code/no-code com extensão por código.

O objetivo não é apenas mostrar workflows. Cada projeto parte de um problema de negócio, documenta decisões técnicas e inclui material para reprodução e demonstração.

## Segurança

Nenhuma credencial, token, senha, Client Secret ou API key deve ser versionada neste repositório. Integrações externas usam placeholders e o gerenciador de credenciais do n8n.
