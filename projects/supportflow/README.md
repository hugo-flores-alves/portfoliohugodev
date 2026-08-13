# SupportFlow — Automated Support Triage

O **SupportFlow** é o terceiro projeto do portfólio FlowForge Automations. Ele recebe solicitações de suporte, transforma a mensagem em um ticket estruturado, classifica categoria e prioridade, registra o chamado no Google Sheets e alerta o time quando o caso é crítico.

## Objetivo

Automatizar a triagem inicial de atendimento sem depender de análise manual para cada solicitação.

## Fluxo

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

## Estrutura da planilha

A aba `Tickets` utiliza:

```text
Ticket | Nome | Email | Empresa | Assunto | Mensagem | Categoria | Prioridade | Status | CriadoEm
```

## Categorias

- `Financeiro`
- `Técnico`
- `Comercial`
- `Outros`

## Prioridades

- `Baixa`
- `Média`
- `Alta`
- `Crítica`

## Regras do MVP

O primeiro MVP usa regras determinísticas de palavras-chave. Isso torna o comportamento fácil de testar, explicar e auditar.

Exemplos:

- termos relacionados a cobrança, boleto, pagamento ou nota fiscal → `Financeiro`;
- erro, sistema, acesso, senha, integração, API ou indisponibilidade → `Técnico`;
- orçamento, proposta, plano, preço ou contratação → `Comercial`;
- ausência de correspondência → `Outros`.

Prioridade sobe conforme sinais de impacto e urgência, como `urgente`, `parado`, `fora do ar`, `não funciona`, `produção`, `hoje` ou `bloqueado`.

## Payload de exemplo

```json
{
  "name": "João Silva",
  "email": "joao@empresa.com",
  "company": "TechStore",
  "subject": "Sistema fora do ar",
  "message": "Nosso sistema está parado e precisamos emitir notas fiscais hoje."
}
```

## Segurança

O workflow versionado não contém URL privada da planilha, credencial Google, token do Telegram ou Chat ID. Esses dados devem ser configurados apenas dentro do n8n.
