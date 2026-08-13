# LeadFlow — Automated Lead Qualification

Um workflow n8n para receber leads por webhook, validar e normalizar os dados, calcular um **lead score** e devolver uma resposta estruturada para o sistema de origem.

## Problema

Formulários de contato normalmente geram dados inconsistentes e obrigam uma pessoa a analisar cada lead manualmente antes de decidir sua prioridade.

## Solução

O LeadFlow transforma a entrada bruta em um lead estruturado e qualificado automaticamente.

```text
Form / App
    ↓
Webhook
    ↓
Validate & Normalize
    ↓
Lead Scoring
    ↓
Structured Response
```

## Regras do MVP

O workflow:

1. recebe `name`, `email`, `company`, `interest` e `employees`;
2. normaliza nome e e-mail;
3. valida campos obrigatórios e formato básico do e-mail;
4. atribui pontos conforme tamanho da empresa e interesse;
5. classifica o lead como `cold`, `warm` ou `hot`;
6. devolve JSON com os dados processados.

### Lead score

- Empresa com 10+ funcionários: +20
- Empresa com 50+ funcionários: +20 adicionais
- Interesse em `automation`: +30
- Interesse em `ai`: +30
- Interesse em `integration`: +25

Classificação:

- `0–29`: cold
- `30–59`: warm
- `60+`: hot

## Como testar

1. Importe `workflow.json` no n8n.
2. Abra o node **Webhook - New Lead**.
3. Execute o workflow em modo de teste.
4. Envie um `POST` para a URL de teste usando o conteúdo de `sample-payload.json`.
5. Confira o JSON retornado.

## Próximas evoluções

- persistência em Google Sheets ou PostgreSQL;
- detecção de duplicados;
- alerta automático para leads `hot`;
- integração com CRM;
- follow-up automático;
- dashboard de conversão.

## Segurança

Este MVP não contém credenciais. Ao adicionar serviços externos, configure as credenciais pelo gerenciador de credenciais do n8n e nunca diretamente no workflow exportado.
