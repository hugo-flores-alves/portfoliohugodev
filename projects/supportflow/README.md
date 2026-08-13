# SupportFlow — Automated Support Triage

Automação de triagem de suporte construída com **n8n**, Google Sheets, Telegram e IA via OpenRouter.

O SupportFlow recebe solicitações de atendimento, transforma cada mensagem em um ticket estruturado, classifica categoria e prioridade, registra o chamado e alerta o time quando o caso exige atenção imediata.

## Problema de negócio

Equipes de suporte perdem tempo lendo manualmente cada solicitação antes mesmo de começar o atendimento. Isso atrasa a identificação de incidentes críticos e aumenta o risco de chamados importantes ficarem misturados com dúvidas simples.

O SupportFlow automatiza essa triagem inicial.

## Versão 1 — Regras determinísticas

```text
Webhook
  ↓
Validate & Normalize
  ↓
Classify Ticket
  ↓
Append Ticket → Google Sheets
  ↓
Critical Ticket?
  ├─ SIM → Telegram Alert
  └─ NÃO → Final Response
```

A primeira versão usa regras de palavras-chave para definir categoria e prioridade. Isso torna o comportamento simples de explicar, testar e auditar.

### Categorias

- `Financeiro`
- `Técnico`
- `Comercial`
- `Outros`

### Prioridades

- `Baixa`
- `Média`
- `Alta`
- `Crítica`

### Exemplos de regras

- cobrança, boleto, pagamento, fatura e reembolso → `Financeiro`;
- erro, sistema, acesso, senha, integração, API ou indisponibilidade → `Técnico`;
- orçamento, proposta, plano, preço ou contratação → `Comercial`;
- ausência de correspondência → `Outros`.

A prioridade sobe conforme sinais de impacto e urgência, como `urgente`, `parado`, `fora do ar`, `produção`, `hoje`, `bloqueado` ou `não funciona`.

## Versão 2 — Classificação com IA

A versão AI V2 substitui a classificação fixa por uma chamada HTTP ao OpenRouter:

```text
Webhook
  ↓
Validate & Normalize
  ↓
AI Classifier - OpenRouter
  ↓
Parse AI Response
  ↓
Append AI Ticket → Google Sheets
  ↓
Critical AI Ticket?
  ├─ SIM → Telegram Alert
  └─ NÃO → Final Response
```

A IA analisa o significado completo do assunto e da mensagem, em vez de depender apenas de palavras isoladas.

A resposta esperada do modelo segue JSON estruturado:

```json
{
  "categoria": "Técnico",
  "prioridade": "Crítica",
  "resumo": "Falha em produção bloqueando a operação",
  "motivo": "Incidente técnico com impacto operacional imediato"
}
```

O node `Parse AI Response` valida e normaliza a saída antes de seguir para os próximos passos. Isso reduz o risco de um formato inesperado quebrar o workflow.

## Estrutura da planilha

A aba `Tickets` utiliza:

```text
Ticket | Nome | Email | Empresa | Assunto | Mensagem | Categoria | Prioridade | Status | CriadoEm
```

A AI V2 também trabalha internamente com `Resumo` e `MotivoIA`, usados principalmente na resposta e no alerta do Telegram.

## Payload de exemplo

```json
{
  "name": "Marcos Rocha",
  "email": "marcos@empresa.com",
  "company": "Alpha Sistemas",
  "subject": "Integração falhando em produção",
  "message": "Nossa API parou de sincronizar os pedidos e a operação está bloqueada hoje."
}
```

## Cenários validados

### Ticket crítico por regras

Um chamado técnico com sistema indisponível foi classificado como `Técnico` e `Crítica`, registrado no Google Sheets e gerou alerta no Telegram.

### Ticket comum

Um chamado comercial de baixa urgência foi registrado normalmente e seguiu pelo ramo não crítico, sem alerta de emergência.

### Primeira chamada de IA

O n8n autenticou com sucesso no OpenRouter via Header Auth e recebeu uma classificação estruturada de categoria e prioridade.

### AI V2 integrada

A versão AI V2 foi preparada para combinar classificação semântica, parsing de JSON, persistência e roteamento crítico no mesmo workflow.

## Comparação das versões

| Aspecto | V1 — Regras | V2 — IA |
|---|---|---|
| Custo do classificador | Zero | Pode usar tier gratuito |
| Explicabilidade | Muito alta | Média |
| Ambiguidade de texto | Limitada | Melhor |
| Dependência externa | Baixa | OpenRouter/modelo remoto |
| Auditoria | Simples | Requer registrar saída da IA |
| Melhor uso | Regras previsíveis | Mensagens variadas e ambíguas |

## Tecnologias

- n8n
- Docker
- JavaScript / Code Node
- Webhooks / HTTP
- Google Sheets API
- Google OAuth 2.0
- Telegram Bot API
- OpenRouter API
- LLM via modelo gratuito em nuvem

## Competências demonstradas

- automação de atendimento;
- triagem e priorização de tickets;
- integração de APIs REST com autenticação por header;
- uso de IA externa sem depender de node proprietário;
- prompt estruturado;
- parsing e validação de resposta de LLM;
- fallback de valores inválidos;
- persistência em Google Sheets;
- roteamento condicional;
- alertas críticos via Telegram;
- comparação entre regras determinísticas e IA.

## Arquivos

```text
supportflow/
├── README.md
├── workflow.json
└── workflow-ai-v2.json
```

## Segurança

Os workflows versionados não contêm API key do OpenRouter, token do Telegram, Chat ID, credenciais Google ou URL privada da planilha. A chave do OpenRouter deve ser armazenada em uma credencial `Header Auth` no n8n usando o header `Authorization: Bearer ...`.

## Limites da versão com IA

O tier gratuito e os modelos gratuitos do OpenRouter podem ter limites e disponibilidade variável. Em produção, seria recomendável definir modelo, orçamento, timeout, retry policy, fallback determinístico e observabilidade das respostas do classificador.
