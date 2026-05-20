---
name: luz-secure-code-review
description: Review code, configuration, architecture, infrastructure, containers, Kubernetes manifests, dependencies, CI/CD, and pipelines for AppSec, DevSecOps, OWASP ASVS, OWASP Top 10, OWASP API Security Top 10, Security by Design, authentication, authorization, input validation, injection, XSS, CSRF, SSRF, CORS, JWT, secrets, logging, headers, rate limits, SAST, DAST, dependency scanning, secret scanning, SBOM, and container scanning risks. Use when the user asks for security review, secure code review, AppSec review, OWASP review, DevSecOps review, hardening, vulnerability analysis, or when a change clearly touches security-sensitive behavior. Do not use for generic code review without security impact.
---

# Luz Secure Code Review

## Purpose

Use this skill to perform a technical secure code review focused on application security, Security by Design, DevSecOps, and OWASP-aligned practices.

Report evidence-backed vulnerabilities and practical hardening opportunities. Do not invent issues. If evidence is incomplete, mark the item as "ponto de atenção" and state what must be checked.

## Review Workflow

1. Identify the stack, trust boundaries, entry points, authentication model, authorization model, data stores, external services, infrastructure, and deployment pipeline.
2. Inspect security-sensitive code paths: controllers/routes, middleware/filters, services/use cases, repositories, template rendering, file handling, HTTP clients, auth/session/JWT logic, CORS, error handlers, and logging.
3. Review configuration: environment variables, framework security config, headers, cookies, TLS assumptions, secrets handling, Dockerfiles, Compose files, Kubernetes manifests, CI/CD, dependency management, and scanning tools.
4. Map findings to OWASP categories when possible, especially OWASP ASVS, OWASP Top 10, and OWASP API Security Top 10.
5. Prioritize exploitable, high-impact risks over theoretical hardening. Include line-specific evidence and realistic impact.
6. Recommend small, testable fixes that preserve behavior and strengthen the security boundary.
7. Suggest tools only when applicable to the repo and task; do not require heavyweight tooling for a narrow code change.

Use current official references when standards/version accuracy matters. Baseline references include OWASP ASVS, OWASP Top 10, OWASP API Security Top 10, OWASP Cheat Sheet Series, and vendor/framework security documentation.

## Risks To Check

Review for these classes when relevant to the codebase:

- Broken Access Control, IDOR, BOLA, tenant/user scoping bugs
- authentication bypass, weak session management, unsafe password handling
- authorization missing in use cases, controllers, resolvers, queues, jobs, or admin paths
- SQL/NoSQL/LDAP/command/template injection
- XSS, unsafe HTML rendering, unsafe markdown, DOM injection
- CSRF on cookie-authenticated state-changing requests
- SSRF, unsafe redirects, unbounded outbound URLs, metadata service access
- permissive CORS, exposed credentials, wildcard origins with credentials
- missing or weak security headers, CSP gaps, clickjacking exposure
- missing rate limits or lockout on login, OTP, password reset, expensive APIs
- stack traces or internal errors exposed to clients
- logs containing passwords, tokens, secrets, session IDs, authorization headers, or sensitive data
- insecure JWT validation, weak algorithms, missing issuer/audience/expiry checks
- refresh tokens stored or rotated unsafely
- hardcoded secrets, committed credentials, keys, certificates, `.env` files
- vulnerable dependencies, unpinned critical dependencies, abandoned packages
- insecure Docker images, root user without need, broad filesystem permissions, missing health checks
- Kubernetes RBAC too broad, secrets in manifests, missing NetworkPolicy, missing resource limits, missing probes
- CI/CD gaps: no SAST, DAST, dependency scanning, secrets scanning, SBOM, or container scanning where appropriate

## Tooling Suggestions

Suggest or run tools only when appropriate for the user's request, repo, and available environment:

- Semgrep or CodeQL for SAST
- SonarQube for maintainability/security gates
- OWASP ZAP for DAST
- Nuclei for template-based exposure checks
- Trivy for dependencies, containers, IaC, and Kubernetes manifests
- Gitleaks for secret scanning
- Syft or CycloneDX for SBOM generation
- Checkov for IaC and Kubernetes policy checks
- Dependabot or Renovate for dependency update automation

If a tool cannot be run, say why and provide the exact command or integration point a maintainer can use.

## Severity Guide

- Crítico: unauthenticated or low-privilege path to full compromise, mass data access, RCE, secret/key exposure, auth bypass, or exploitable injection in a sensitive path.
- Alto: broken authorization on user/tenant resources, exploitable SSRF, weak JWT/session handling, stored XSS, critical dependency exposure, or CI/CD/deployment weakness that can plausibly lead to compromise.
- Médio: reflected XSS with constraints, missing rate limits on sensitive endpoints, overly broad CORS without proven exploitation, missing headers, partial logging exposure, or container/IaC hardening gaps.
- Baixo: defense-in-depth improvement, documentation/test gap, minor configuration hardening, or low-impact dependency hygiene.

Set the overall security risk from the highest credible vulnerability, adjusted by exploitability, exposure, privilege required, affected assets, and compensating controls.

## Evidence Rules

- Include file paths and line numbers whenever possible.
- Show the minimum code or config snippet needed to prove the finding.
- Avoid reporting a vulnerability based only on a keyword; trace how input reaches the sink or how access is controlled.
- Separate confirmed findings from assumptions and points of attention.
- Do not print real secrets. If a secret appears in code, mask it in the report and advise rotation.

## Required Output

Respond in the user's language unless they ask otherwise. Use this structure:

### 1. Resumo de Segurança

- Risco geral: Baixo, Médio, Alto ou Crítico
- Principais vulnerabilidades
- Impacto técnico

### 2. Achados Detalhados

For each finding:

- Título
- Severidade
- Categoria OWASP/AppSec
- Evidência no código
- Risco prático
- Recomendação objetiva
- Exemplo de correção quando possível

When there are no confirmed findings, say so clearly and list remaining test/tooling gaps.

### 3. Checklist de Segurança

Mark each item as OK, Risco, Ponto de atenção, or Não avaliado:

- Autenticação correta
- Autorização por recurso
- Validação de entrada
- Logs seguros
- Secrets protegidos
- Headers seguros
- CORS restrito
- Rate limit aplicado
- Dependências escaneadas
- Containers escaneados
- Pipeline DevSecOps
- SBOM gerado
- Kubernetes com menor privilégio

## Correction Guidance

Prefer focused fixes that protect the boundary being reviewed:

- Put authorization close to the use case/resource and test owner/tenant mismatches.
- Validate external input with schemas, allowlists, length limits, and safe parsers.
- Use parameterized queries and safe ORM APIs; avoid string-built queries.
- Encode output by context and sanitize user-controlled HTML only with proven libraries.
- Require CSRF protection for cookie-authenticated state changes.
- Restrict outbound requests and block internal metadata/private network targets for user-controlled URLs.
- Restrict CORS to known origins; do not combine wildcard origins with credentials.
- Add secure headers and CSP based on the frontend's actual needs.
- Redact tokens, passwords, authorization headers, cookies, and secrets from logs.
- Validate JWT issuer, audience, algorithm, signature, expiry, and key rotation.
- Store refresh tokens in protected storage, rotate them, and revoke on suspicious reuse.
- Move secrets to secret managers or CI variables and rotate exposed values.
- Run containers as non-root when feasible and set resource limits/probes in orchestration.
- Add scanning gates incrementally where the repo lacks coverage.

## Example Finding

```markdown
### Endpoint permite IDOR em recurso de aluno
- Severidade: Alto
- Categoria OWASP/AppSec: Broken Access Control / OWASP API Security BOLA
- Evidência no código: `StudentController#getById(Long id)` busca o aluno por ID e retorna o DTO sem validar se o usuário autenticado pode acessar esse aluno.
- Risco prático: um usuário autenticado pode trocar o ID na URL e acessar dados de outro titular.
- Recomendação objetiva: validar escopo de usuário/tenant no use case ou na consulta, e adicionar teste cobrindo acesso negado para recurso de outro usuário.
- Exemplo de correção: trocar `findById(id)` por `findByIdAndUserScope(id, currentUser.scope())` e retornar 403/404 quando fora do escopo.
```
