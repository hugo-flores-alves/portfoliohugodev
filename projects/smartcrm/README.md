# SmartCRM — Automated Sales Follow-up

O **SmartCRM** é a segunda automação do portfólio FlowForge Automations. Ele continua o trabalho do LeadFlow: depois que um lead é capturado e qualificado, o SmartCRM acompanha o estágio comercial e organiza os próximos contatos.

## Objetivo

Transformar a planilha `LeadFlow CRM` em um CRM leve e automatizado.

O projeto será dividido em dois workflows:

1. **Update Lead Stage** — recebe uma atualização comercial, localiza o lead pelo e-mail e atualiza status, último contato, próximo follow-up e observações.
2. **Follow-up Monitor** — roda em agenda, encontra follow-ups vencidos e envia alertas no Telegram.

## Pipeline comercial

```text
Novo → Contato → Reunião → Proposta → Ganho / Perdido
```

## Colunas adicionais necessárias

Na aba `Leads`, adicionar após `Data`:

```text
Status | UltimoContato | ProximoFollowUp | Observacoes
```

A estrutura completa fica:

```text
Nome | Email | Empresa | Interesse | Funcionarios | Score | Classificacao | Data | Status | UltimoContato | ProximoFollowUp | Observacoes
```

## Workflow 1 — Update Lead Stage

```text
Webhook
  ↓
Validate CRM Update
  ↓
Find Lead by Email
  ↓
Lead Exists?
  ├─ NÃO → Not Found Response
  └─ SIM → Build Updated Record
              ↓
          Update Lead
              ↓
          Update Response
```

### Payload de exemplo

```json
{
  "email": "maria.teste@empresa.com",
  "status": "Contato",
  "followUpDays": 2,
  "observacoes": "Cliente pediu retorno após avaliar a proposta."
}
```

## Regras

Status permitidos:

- `Novo`
- `Contato`
- `Reunião`
- `Proposta`
- `Ganho`
- `Perdido`

`followUpDays` define quantos dias após o contato o próximo follow-up deve acontecer. Para `Ganho` e `Perdido`, o próximo follow-up fica vazio.

## Segurança

O workflow versionado não contém credenciais Google, URLs privadas, tokens do Telegram ou Chat IDs. Esses dados devem ser configurados apenas dentro do n8n.
