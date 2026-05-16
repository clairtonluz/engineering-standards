---
name: luz-software-design-patterns-decision-guide
description: Decision guide for selecting the right reusable software design pattern skill. Use when unsure whether to apply Singleton, Factory, Builder, Strategy, State, Clean Architecture, Hexagonal Architecture, CQRS, Event Sourcing, Saga, Outbox, Retry, Circuit Breaker, Repository, DAO, Data Mapper, or related patterns.
---

# Software Design Patterns Decision Guide

## First Question
Name the force creating complexity:
- Object creation or lifecycle: start with `$luz-factory-method`, `$luz-abstract-factory`, `$luz-builder`, `$luz-prototype`, or `$luz-singleton`.
- Interface mismatch or composition: start with `$luz-adapter`, `$luz-facade`, `$luz-decorator`, `$luz-proxy`, `$luz-bridge`, `$luz-composite`, or `$luz-flyweight`.
- Runtime behavior variation: start with `$luz-strategy`, `$luz-state`, `$luz-command`, `$luz-observer`, `$luz-mediator`, or `$luz-chain-of-responsibility`.
- Business model boundaries: start with `$luz-entity`, `$luz-value-object`, `$luz-aggregate`, `$luz-aggregate-root`, `$luz-repository`, `$luz-domain-service`, or `$luz-application-service`.
- Application architecture: start with `$luz-layered-architecture`, `$luz-hexagonal-architecture`, `clean-architecture`, `$luz-onion-architecture`, or `$luz-modular-monolith`.
- Distributed workflow or messaging: start with `$luz-event-driven-architecture`, `$luz-message-queue`, `$luz-publish-subscribe`, `$luz-saga`, `$luz-outbox`, or `$luz-inbox`.
- Reliability problem: start with `$luz-timeout`, `$luz-retry`, `$luz-circuit-breaker`, `$luz-bulkhead`, `$luz-fallback`, `$luz-rate-limiting`, `$luz-load-shedding`, `$luz-health-check`, or `$luz-backpressure`.
- Persistence problem: start with `$luz-repository`, `$luz-dao`, `$luz-data-mapper`, `$luz-unit-of-work`, `$luz-identity-map`, `$luz-lazy-loading`, `$luz-cache-aside`, or `$luz-write-through-cache`.
- Frontend state or composition: start with `$luz-component-pattern`, `$luz-container-presenter`, `$luz-flux`, `$luz-redux`, `$luz-mvvm`, or `$luz-atomic-design`.
- Cloud or Kubernetes runtime behavior: start with `$luz-sidecar`, `$luz-ambassador`, `$luz-init-container`, `$luz-operator`, `$luz-service-mesh`, or `$luz-leader-election`.
- Security boundary: start with `$luz-oauth2-authorization-code`, `$luz-bff-token-handler`, `$luz-token-exchange`, `$luz-zero-trust-architecture`, `$luz-api-gateway-security`, or `$luz-strangler-security-migration`.

## Common Pattern Choices
- Factory Method vs Abstract Factory vs Builder: use `$luz-factory-method` for one product type, `$luz-abstract-factory` for compatible product families, and `$luz-builder` for complex object construction with optional values or validation.
- Strategy vs State: use `$luz-strategy` when the algorithm varies independently of object lifecycle; use `$luz-state` when the valid behavior changes because the object moved through lifecycle states.
- Adapter vs Facade vs Proxy: use `$luz-adapter` to translate contracts, `$luz-facade` to simplify a subsystem, and `$luz-proxy` to control access to the same contract.
- Decorator vs Proxy: use `$luz-decorator` to add composable behavior; use `$luz-proxy` when the wrapper represents access control, remote access, lazy loading, or protection.
- Chain of Responsibility vs Pipe and Filter: use `$luz-chain-of-responsibility` when handlers may short-circuit or pass a request along; use `$luz-pipe-and-filter` when each stage transforms data for the next stage.
- Hexagonal vs Clean vs Onion: use `$luz-hexagonal-architecture` when ports and adapters are the key design tool, `clean-architecture` when use-case boundaries and dependency direction are central, and `$luz-onion-architecture` when the domain model is the innermost organizing force.
- CQRS vs Event Sourcing: use `$luz-cqrs` when reads and writes need different models; use `$luz-event-sourcing` only when events are the source of truth and replay/audit value justifies the cost.
- Microservices vs Modular Monolith: use `$luz-modular-monolith` until independent deployment, ownership, and scaling are worth distributed-system complexity.
- Saga vs Outbox vs Inbox: use `$luz-saga` to coordinate a distributed workflow, `$luz-outbox` to reliably publish after local commits, and `$luz-inbox` to deduplicate consumed messages.
- Retry vs Timeout vs Circuit Breaker vs Bulkhead: set `$luz-timeout` first, use `$luz-retry` only for safe transient failures, add `$luz-circuit-breaker` to fail fast during dependency outages, and use `$luz-bulkhead` to isolate resource pools.
- Repository vs DAO vs Data Mapper vs Active Record: use `$luz-repository` for aggregate access, `$luz-dao` for storage-oriented operations, `$luz-data-mapper` to keep domain objects persistence-free, and `$luz-active-record` for simple data-centric CRUD.
- Observer vs Publish-Subscribe vs Event Bus: use `$luz-observer` in-process, `$luz-publish-subscribe` across subscribers through infrastructure, and `$luz-event-bus` when a shared event mechanism is needed and governed.

## Do Not Apply A Pattern When
- The direct implementation is shorter and clearer.
- There is only one variation and no real boundary.
- The pattern would hide validation, authorization, persistence, network, or failure behavior.
- Tests would need to know more implementation details after the pattern is added.

## Output Guidance
When recommending a pattern:
1. State the problem force.
2. Name the selected skill.
3. Mention one rejected alternative and why.
4. Keep the implementation scoped to the smallest useful version.
