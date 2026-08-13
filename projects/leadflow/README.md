# LeadFlow — Automated Lead Qualification

Automação de qualificação de leads construída com **n8n**, Google Sheets e Telegram.

O projeto recebe leads via webhook, valida e normaliza os dados, calcula um lead score, evita cadastros duplicados, registra novos leads em uma planilha e envia um alerta instantâneo no Telegram quando um lead é classificado como `hot`.

## Problema de negócio

Formulários comerciais costumam gerar três problemas recorrentes:

- dados inconsistentes;
- leads duplicados no CRM;
- demora para identificar oportunidades de maior prioridade.

O LeadFlow automatiza essas decisões antes que o lead chegue ao time comercial.

## Solução

```text
Form / Website
      ↓ POST JSON
Webhook - New Lead
      ↓
Validate & Normalize
      ↓
Lead Scoring
      ↓
Find Existing Email
      ↓
Email Already Exists?
   ┌──┴──┐
  SIM   NÃO
   │     ↓
   │  Append New Lead → Google Sheets
   │     ↓
   │  Hot Lead?
   │   ┌─┴─┐
   │  SIM NÃO
   │   │   │
   │   ▼   ▼
   │ Telegram  Finaliza
   ▼
Duplicate Response
```

## Funcionalidades implementadas

- Webhook HTTP `POST` para entrada de leads.
- Validação de nome e e-mail.
- Normalização de campos antes da lógica de negócio.
- Lead scoring determinístico.
- Classificação automática como `cold`, `warm` ou `hot`.
- Busca do e-mail no Google Sheets antes da inserção.
- Bloqueio de leads duplicados.
- Persistência de novos leads no Google Sheets.
- Alerta automático via Telegram para leads `hot`.
- Respostas distintas para lead novo e lead duplicado.

## Lead scoring

| Regra | Pontos |
|---|---:|
| Empresa com 10+ funcionários | +20 |
| Empresa com 50+ funcionários | +20 adicionais |
| Interesse em `automation` | +30 |
| Interesse em `ai` | +30 |
| Interesse em `integration` | +25 |

Classificação:

- `0–29`: `cold`
- `30–59`: `warm`
- `60+`: `hot`

## Estrutura da planilha

A aba `Leads` utiliza as colunas:

```text
Nome | Email | Empresa | Interesse | Funcionarios | Score | Classificacao | Data
```

## Payload de exemplo

```json
{
  "name": "Maria Oliveira",
  "email": "maria.teste@empresa.com",
  "company": "NovaTech",
  "interest": "automation",
  "employees": 100
}
```

## Cenários validados

### Novo lead

Um e-mail ainda inexistente é processado, pontuado e salvo no Google Sheets.

### Lead duplicado

Ao enviar o mesmo e-mail novamente, o workflow localiza o registro existente e não cria uma segunda linha.

### Lead HOT

Um novo lead com score `60+` é salvo e dispara uma notificação automática para o Telegram.

## Como executar

1. Execute uma instância do n8n.
2. Importe `workflow-v2.json`.
3. Configure uma credencial Google Sheets via OAuth2.
4. Substitua o placeholder do documento pela URL da sua planilha.
5. Use a aba `Leads` e os cabeçalhos mostrados acima.
6. Configure a credencial do Telegram usando um bot criado com o BotFather.
7. Defina seu `Chat ID` apenas dentro do n8n.
8. Execute o workflow em modo de teste e envie um `POST` ao webhook.

> O arquivo versionado não contém tokens, Client Secrets, Chat IDs nem URLs privadas.

## Tecnologias

- n8n
- Docker
- JavaScript / Code Node
- Webhooks / HTTP
- Google Sheets API
- Google OAuth 2.0
- Telegram Bot API

## Competências demonstradas

Este projeto demonstra integração entre APIs e serviços, transformação de dados, regras de negócio, idempotência por deduplicação, persistência, roteamento condicional, notificações e gerenciamento seguro de credenciais.

## Próximas evoluções

- autenticação do webhook;
- tratamento centralizado de erros;
- retry policy;
- PostgreSQL/Supabase como persistência alternativa;
- integração com CRM;
- follow-up automático;
- métricas e dashboard de conversão.

## Segurança

Credenciais devem permanecer no gerenciador de credenciais do n8n. Nunca publique tokens do Telegram, Client Secrets do Google, Chat IDs privados ou credenciais dentro do JSON exportado.
