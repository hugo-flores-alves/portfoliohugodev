# SmartCRM — Automated Sales Follow-up

Automação de pipeline e follow-up comercial construída com **n8n**, Google Sheets, JavaScript e Telegram.

O SmartCRM continua o trabalho do LeadFlow: depois que um lead é capturado e qualificado, ele permite atualizar o estágio comercial e monitora automaticamente contatos que ficaram pendentes.

## Problema de negócio

Capturar leads não resolve o processo comercial inteiro. Depois da entrada do contato, equipes ainda precisam saber:

- em qual etapa cada oportunidade está;
- quando aconteceu o último contato;
- quando é necessário falar novamente com o lead;
- quais follow-ups estão atrasados.

O SmartCRM automatiza esse acompanhamento usando uma planilha como CRM leve.

## Arquitetura

O projeto possui dois workflows independentes e complementares.

### Workflow 1 — Update Lead Stage

```text
Webhook - Update Lead
        ↓
Validate CRM Update
        ↓
Find Lead by Email
        ↓
    Lead Exists?
      /       \
    NÃO       SIM
     ↓         ↓
Not Found   Build Updated Record
Response          ↓
              Update Lead
                  ↓
             Update Response
```

Ele recebe uma atualização comercial, encontra o lead existente pelo e-mail e altera a mesma linha no Google Sheets, evitando criar outro cadastro.

### Workflow 2 — Follow-up Monitor

```text
Daily Follow-up Check
        ↓
Read CRM Leads
        ↓
Find Overdue Follow-ups
        ↓
Follow-up Due?
        ↓ SIM
Send Follow-up Alert
        ↓
Alert Logged
```

O monitor lê os leads periodicamente, ignora oportunidades encerradas (`Ganho` e `Perdido`) e identifica datas de follow-up iguais ou anteriores ao momento da execução. Cada pendência gera um alerta no Telegram.

## Pipeline comercial

```text
Novo → Contato → Reunião → Proposta → Ganho / Perdido
```

## Estrutura da planilha

A aba `Leads` utiliza:

```text
Nome | Email | Empresa | Interesse | Funcionarios | Score | Classificacao | Data | Status | UltimoContato | ProximoFollowUp | Observacoes
```

## Atualização de estágio

Payload de exemplo:

```json
{
  "email": "maria.teste@empresa.com",
  "status": "Contato",
  "followUpDays": 2,
  "observacoes": "Cliente pediu retorno em dois dias."
}
```

O workflow atualiza automaticamente:

- `Status`;
- `UltimoContato`;
- `ProximoFollowUp`;
- `Observacoes`.

Status permitidos:

- `Novo`
- `Contato`
- `Reunião`
- `Proposta`
- `Ganho`
- `Perdido`

Para `Ganho` e `Perdido`, o próximo follow-up pode permanecer vazio.

## Compatibilidade de datas

O Follow-up Monitor trata datas retornadas pelo Google Sheets em diferentes representações, incluindo formato brasileiro `DD/MM/AAAA HH:mm`, ISO e serial numérico de planilha.

Isso evita que uma data válida seja ignorada apenas por diferença de formatação.

## Alerta do Telegram

Quando uma pendência é detectada, a automação envia informações comerciais relevantes, como nome, empresa, e-mail, status, classificação, score, data do próximo follow-up e observações.

## Cenários validados

### Atualização de lead existente

Um lead previamente criado pelo LeadFlow foi localizado pelo e-mail e teve status, último contato, próximo follow-up e observações atualizados na mesma linha.

### Follow-up vencido

Um lead com `Status = Contato` e uma data de `ProximoFollowUp` no passado foi identificado pelo monitor.

### Alerta automático

O lead vencido seguiu pelo fluxo de pendência e o alerta correspondente foi recebido com sucesso no Telegram.

## Como executar

1. Tenha uma instância do n8n disponível.
2. Importe `workflow-update-stage.json`.
3. Configure a credencial Google Sheets e a aba `Leads`.
4. Teste a atualização usando o webhook.
5. Importe `workflow-followup-monitor.json`.
6. Configure Google Sheets no node de leitura.
7. Configure sua credencial Telegram e Chat ID no node de alerta.
8. Defina o horário desejado no Schedule Trigger.
9. Ative o workflow para execução automática.

## Tecnologias

- n8n
- Docker
- JavaScript / Code Node
- Webhooks / HTTP
- Google Sheets API
- Google OAuth 2.0
- Telegram Bot API
- Schedule Trigger

## Competências demonstradas

- automação de pipeline comercial;
- atualização de registros existentes;
- busca e matching por identificador;
- lógica temporal e processamento de datas;
- execução agendada;
- roteamento condicional;
- notificações orientadas a eventos;
- integração entre workflows;
- gerenciamento seguro de credenciais.

## Relação com o LeadFlow

```text
LeadFlow
Captura → Validação → Scoring → Deduplicação → Persistência
                                      ↓
                                  SmartCRM
                        Pipeline → Follow-up → Alerta
```

Os dois projetos juntos simulam uma pequena operação comercial automatizada.

## Próximas evoluções

- histórico de interações por lead;
- responsável comercial por oportunidade;
- dashboard de pipeline;
- atualização por formulário ou interface web;
- lembretes escalonados;
- integração com CRM dedicado;
- PostgreSQL/Supabase para maior volume;
- tratamento centralizado de erros e retries.

## Segurança

Os arquivos versionados não devem conter credenciais Google, URLs privadas de planilhas, tokens do Telegram, Chat IDs, Client Secrets ou outras informações sensíveis. Esses valores ficam no gerenciador de credenciais/configuração local do n8n.
