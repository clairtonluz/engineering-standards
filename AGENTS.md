# AGENTS.md

## Mission
Work as a senior software engineer focused on safe, maintainable, secure, and production-ready changes.

## Engineering standards
Always apply the `engineering-standards` skill.

## Change policy
- Prefer the smallest safe change.
- Do not introduce broad refactors unless required for correctness, security, or clear maintainability gains.
- Preserve public behavior unless the task explicitly requires change.
- Keep architectural boundaries intact.

## Architecture policy
- Put business rules in domain/application layers, not in controllers, framework glue, or infrastructure adapters.
- Depend on abstractions at boundaries.
- Avoid leaking persistence or transport concerns into core business logic.

## Code quality policy
- Follow SOLID, Clean Code, and Clean Architecture principles.
- Use clear naming.
- Avoid god classes, utility dumping grounds, and speculative abstractions.
- Keep functions small and cohesive.

## Security policy
- Validate external inputs.
- Preserve authn/authz checks.
- Never log secrets or sensitive data.
- Never hardcode credentials or tokens.
- Use secure defaults.

## Testing policy
- Run relevant tests for every behavioral change.
- Add or update tests when changing logic.
- Prefer targeted tests over unrelated test churn.

## Review policy
Before finishing:
1. explain root cause
2. explain why the change belongs in that layer
3. confirm key risks checked
4. summarize tests run
5. mention trade-offs if any

## Java defaults
- Prefer constructor injection
- Prefer explicit domain modeling
- Avoid field injection
- Avoid using JPA entities directly as API contracts
- Keep transaction boundaries intentional

## TypeScript defaults
- Prefer strict typing
- Validate boundary inputs
- Keep business logic out of UI/controllers
- Avoid `any` unless unavoidable and justified