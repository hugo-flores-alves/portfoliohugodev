# Prospectador AI

Sistema de prospecção B2B construído com **n8n**, **Apify**, **OpenAI** e **Google Sheets** para descobrir empresas no Google Maps, qualificar oportunidades e organizar uma fila comercial.

## Problema

Prospecção manual consome tempo e normalmente gera listas sem prioridade. O objetivo deste projeto é transformar uma busca por nicho e localização em uma base comercial organizada, com critérios objetivos e análise por IA antes da abordagem.

## Fluxo

```text
Nicho + localização
        ↓
Apify / Google Maps
        ↓
Normalização + deduplicação
        ↓
Pré-score e filtro de ICP
        ↓
OpenAI
├─ ICP score
├─ Opportunity score
├─ Score final
├─ Tier A+ / A / B / C / D
├─ Evidências
├─ Oportunidade
└─ Mensagem inicial
        ↓
Google Sheets CRM
        ↓
Fila comercial / aprovação humana
```

## O que o sistema faz

- Pesquisa empresas no Google Maps por nicho e região.
- Normaliza dados e remove registros inadequados ou duplicados.
- Aplica pré-qualificação antes de usar IA.
- Usa OpenAI para avaliar aderência ao ICP e oportunidade comercial.
- Separa `ICP Score` e `Opportunity Score` para não confundir perfil ideal com evidência de abordagem.
- Classifica leads em tiers `A+`, `A`, `B`, `C` e `D`.
- Gera uma hipótese de oportunidade e uma mensagem inicial baseada apenas nos dados disponíveis.
- Registra tudo em um CRM no Google Sheets.
- Mantém aprovação humana antes de qualquer contato comercial.

## Critérios de segurança da IA

O prompt foi desenhado para impedir afirmações sem evidência. A IA não pode afirmar, por exemplo, que uma empresa perde leads, demora a responder ou possui determinado faturamento sem que esses dados estejam disponíveis.

## Stack

- n8n
- Apify Google Maps Scraper
- OpenAI API
- Google Sheets
- JavaScript / Code nodes
- REST APIs
- HTTP Request
- JSON estruturado

## Validação técnica

O MVP foi validado ponta a ponta com uma coleta inicial de estabelecimentos no Rio de Janeiro:

```text
Google Maps → n8n → normalização → scoring → OpenAI → CRM
```

O fluxo retornou telefones, avaliações, categorias, localização e outros sinais comerciais, aplicou classificação por IA e gravou os resultados no CRM.

## Arquitetura comercial

```text
Empresas encontradas
      ↓
ICP compatível?
      ↓
Pré-score
      ↓
Análise por IA
      ↓
A+ / A → aprovação para prospecção
B      → reserva
C / D  → arquivados
```

## Segurança

Nenhuma API key, token, credencial OAuth ou ID privado é versionado neste projeto. As integrações usam placeholders e credenciais configuradas diretamente no n8n.

## Próxima etapa

A etapa seguinte é conectar a fila aprovada ao canal de prospecção, registrar respostas e medir conversão até reunião, proposta e cliente.
