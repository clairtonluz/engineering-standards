---
name: luz-lgpd-code-review
description: Review code, architecture, APIs, databases, logs, integrations, analytics, AI usage, queues, webhooks, backups, and environments for LGPD, Privacy by Design, data minimization, personal data exposure, sensitive data handling, children/adolescent data, retention, anonymization, consent evidence, and third-party sharing risks. Use when the user asks for LGPD, privacy, data protection, PII, personal data, compliance, DPO review support, or when a code review clearly touches personal or sensitive data. Do not use for generic code review without privacy impact.
---

# Luz LGPD Code Review

## Purpose

Use this skill to perform a technical privacy review focused on LGPD, Privacy by Design, and data minimization. Treat legal basis as a point for validation with legal counsel or the DPO; do not decide legal basis alone.

This skill does not replace legal assessment. It identifies technical evidence, privacy risks, and engineering corrections that support LGPD compliance.

## Review Workflow

1. Identify the processing context: feature purpose, actors, data subjects, systems, repositories, data flows, APIs, storage, logs, queues, analytics, AI providers, webhooks, and third parties.
2. Inventory personal data and sensitive data. Include direct identifiers, indirect identifiers, metadata, and inferred identifiers.
3. Trace where data is collected, validated, transformed, stored, returned, logged, exported, shared, retained, deleted, anonymized, or backed up.
4. Compare collected and exposed data with the feature purpose. Flag unnecessary fields, broad DTOs, entity leaks, and accidental response expansion.
5. Review special categories carefully: children and adolescents, health, biometrics, disability, school records, location, photos, documents, and credentials.
6. Check technical controls: access scope, DTO minimization, masking, pseudonymization, encryption, log redaction, retention jobs, deletion/anonymization paths, environment segregation, and test data handling.
7. Report only findings backed by evidence. If the risk is plausible but not proven, mark it as "ponto de atenção" and state what evidence is missing.

Use official/current references when the task requires standards alignment, especially Lei 13.709/2018, ANPD guidance, Privacy by Design, and data protection authority guidance relevant to the repository's jurisdiction.

## Data To Look For

Treat these as personal data or privacy-sensitive signals when linked or linkable to a natural person:

- name, CPF, RG, birth date, gender, parent/guardian data
- email, phone, address, school enrollment, student registration, employee registration
- photo, video, voice, documents, signatures
- IP address, device ID, session ID, cookie ID, push token, browser fingerprint
- geolocation, attendance/location traces, telemetry tied to a user
- health, disability, biometrics, race/ethnicity, religion, political opinion, union affiliation
- child or adolescent data, school records, grades, behavior, family/social context
- tokens, credentials, secrets, recovery codes, authentication factors

## Finding Categories

Use the most specific category that fits:

- Coleta excessiva
- Finalidade indefinida
- Dados pessoais em DTO/API response
- Retorno acidental de entidade completa
- Dados sensíveis ou de menores
- Logs com dados pessoais
- Retenção indefinida
- Falta de exclusão ou anonimização
- Consentimento ou aceite sem evidência técnica
- Compartilhamento com terceiros
- Integração externa, analytics, IA, webhook ou fila
- Transferência internacional de dados
- Ambiente não produtivo com dados reais
- Backup, dump ou exportação com dados pessoais
- Controle de acesso insuficiente para dados pessoais

## Severity Guide

- Crítico: exposição ampla ou pública de dados pessoais/sensíveis, dados de menores sem proteção, vazamento em logs/exports/backups acessíveis, integração externa sem controle técnico, ou ausência de autorização para dados altamente sensíveis.
- Alto: minimização falha com dados sensíveis, entidade completa retornada por API, retenção indefinida relevante, logs persistentes com PII, ou ambiente não produtivo usando dados reais sem proteção.
- Médio: campos pessoais desnecessários em DTOs internos, ausência de mascaramento em telas/admin, consentimento/aceite sem trilha técnica clara, ou exclusão/anonymization incompleta.
- Baixo: ajustes localizados de nomenclatura, documentação técnica, testes, ou pequenas melhorias de minimização com baixo impacto.

Classify the overall LGPD risk from the highest credible finding, adjusted by data volume, sensitivity, affected subjects, exposure path, and exploitability.

## Evidence Rules

- Include file paths and line numbers whenever possible.
- Quote or summarize the smallest code fragment needed to prove the issue.
- Do not infer a data flow from naming alone when the code contradicts it.
- Distinguish confirmed findings from assumptions.
- Do not expose real personal data in the report; mask values if examples contain PII.

## Required Output

Respond in the user's language unless they ask otherwise. Use this structure:

### 1. Resumo LGPD

- Risco geral: Baixo, Médio, Alto ou Crítico
- Principais riscos de privacidade
- Impacto provável
- Limitações: informar que a análise é técnica e não substitui avaliação jurídica

### 2. Achados Detalhados

For each finding:

- Título
- Severidade
- Categoria
- Evidência no código
- Risco LGPD
- Recomendação objetiva
- Exemplo de correção quando possível

When there are no confirmed findings, say so clearly and list only residual risk or test gaps.

### 3. Checklist LGPD

Mark each item as OK, Risco, Ponto de atenção, or Não avaliado:

- Coleta mínima
- Finalidade clara
- Base legal a validar com jurídico/DPO
- DTOs mínimos
- Logs higienizados
- Retenção definida
- Anonimização/exclusão
- Terceiros avaliados
- Dados sensíveis protegidos
- Ambientes não produtivos protegidos

## Correction Guidance

Prefer simple technical fixes that reduce data exposure:

- Replace entity responses with explicit DTOs containing only required fields.
- Add response mappers that intentionally whitelist fields.
- Mask CPF, email, phone, IP, and identifiers in logs and UI where full value is unnecessary.
- Remove personal data from debug logs and exception messages.
- Add retention configuration and scheduled cleanup where retention is required.
- Add anonymization or deletion paths with tests.
- Use synthetic or anonymized data in dev, staging, demos, and automated tests.
- Gate third-party integrations behind clear configuration, data minimization, and documented data sharing.
- Keep consent/terms acceptance evidence as auditable metadata when applicable, without over-collecting.

## Example Finding

```markdown
### Retorno de entidade completa expõe dados de aluno
- Severidade: Alto
- Categoria: Dados pessoais em DTO/API response
- Evidência no código: `StudentController#getStudent` retorna `StudentEntity`, incluindo CPF, telefone e dados do responsável.
- Risco LGPD: expõe mais dados pessoais do que o necessário para a finalidade da tela e aumenta impacto em caso de acesso indevido.
- Recomendação objetiva: retornar um DTO mínimo com apenas os campos usados pelo cliente e testar que CPF/telefone não aparecem na resposta.
- Exemplo de correção: criar `StudentSummaryDTO` e mapear explicitamente `id`, `nomeSocial` e `matricula`.
```
