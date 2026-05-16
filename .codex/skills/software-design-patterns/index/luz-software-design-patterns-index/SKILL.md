---
name: luz-software-design-patterns-index
description: Index of reusable global software design pattern skills. Use when choosing, browsing, or referencing design pattern skills by category, including creational, structural, behavioral, architecture, DDD, concurrency, integration, cloud-native, frontend, security, resilience, and data patterns.
---

# Software Design Patterns Index

Use this skill to find the smallest relevant pattern skill. Prefer one focused skill over loading broad guidance.

## Creational
Use creational patterns when object construction, lifecycle, or families of related objects are the problem.

- `$luz-abstract-factory`: Create families of related objects through one factory interface.
- `$luz-builder`: Construct complex objects step by step while preserving validation and readability.
- `$luz-factory-method`: Define a creation method so callers depend on a product interface instead of concrete classes.
- `$luz-prototype`: Create new objects by copying a configured example instance.
- `$luz-singleton`: Ensure one shared instance is used within a scope, preferably through the runtime or dependency injection container.

## Structural
Use structural patterns when interfaces, object composition, or subsystem boundaries are the problem.

- `$luz-adapter`: Translate one interface or contract into another without changing either side.
- `$luz-bridge`: Separate an abstraction from its implementation so both can vary independently.
- `$luz-composite`: Represent part-whole hierarchies so clients can treat individual and grouped objects uniformly.
- `$luz-decorator`: Wrap an object to add behavior while preserving the same interface.
- `$luz-facade`: Provide a small, simple interface over a larger subsystem.
- `$luz-flyweight`: Share immutable intrinsic state across many fine-grained objects.
- `$luz-proxy`: Control access to another object through the same interface.

## Behavioral
Use behavioral patterns when control flow, algorithms, lifecycle behavior, or collaboration between objects is the problem.

- `$luz-chain-of-responsibility`: Pass a request through a sequence of handlers until one handles it or all contribute.
- `$luz-command`: Represent an operation as an object or message that can be handled, queued, logged, or retried.
- `$luz-interpreter`: Define and evaluate a small language or expression grammar.
- `$luz-iterator`: Expose sequential access to a collection without exposing its internal representation.
- `$luz-mediator`: Centralize coordination between peers so they do not depend on each other directly.
- `$luz-memento`: Capture and restore an object's state without exposing internals.
- `$luz-observer`: Notify interested subscribers when state changes without coupling publisher to concrete subscribers.
- `$luz-state`: Encapsulate behavior that changes with an object's lifecycle state.
- `$luz-strategy`: Select one interchangeable algorithm behind a stable interface.
- `$luz-template-method`: Define an algorithm skeleton in a base class and let subclasses override steps.
- `$luz-visitor`: Add operations over a stable object structure without changing the objects.

## Architecture
Use architectural patterns when module boundaries, deployment shape, system communication, or large-scale change is the problem.

- `$luz-api-gateway`: Provide an edge entry point for routing, protocol translation, and cross-cutting edge policies.
- `$luz-backend-for-frontend`: Provide a backend tailored to a specific frontend experience.
- `$luz-bulkhead`: Isolate resources so failure or saturation in one area does not sink the whole system.
- `$luz-cqrs`: Separate command models that change state from query models that read state.
- `$luz-event-sourcing`: Persist domain events as the source of truth and derive state from event history.
- `$luz-event-driven-architecture`: Coordinate systems by publishing and reacting to events.
- `$luz-hexagonal-architecture`: Place application use cases at the center and connect external systems through ports and adapters.
- `$luz-inbox`: Track consumed messages to make message handling idempotent.
- `$luz-layered-architecture`: Organize code into layers with clear responsibilities and dependency direction.
- `$luz-mvc`: Separate model, view, and controller responsibilities for UI-backed applications.
- `$luz-mvp`: Separate a passive view from a presenter that owns presentation behavior.
- `$luz-mvvm`: Use a view model to expose observable presentation state to a bound view.
- `$luz-microservices`: Split a system into independently deployable services around business capabilities.
- `$luz-modular-monolith`: Build one deployable application with strong internal module boundaries.
- `$luz-onion-architecture`: Keep domain model at the center with dependencies pointing inward through surrounding layers.
- `$luz-outbox`: Persist messages in the same transaction as state changes, then publish them asynchronously.
- `$luz-pipe-and-filter`: Process data through independent filters connected by explicit pipes.
- `$luz-soa`: Expose reusable enterprise services through governed contracts.
- `$luz-saga`: Coordinate a distributed business transaction through local transactions and compensations.
- `$luz-serverless-architecture`: Run application capabilities on managed event-driven compute and services.
- `$luz-strangler-fig`: Replace a legacy system incrementally by routing capabilities to a new implementation.
- `$luz-circuit-breaker`: use `$luz-circuit-breaker`. Canonical skill lives in resilience/luz-circuit-breaker.
- `clean-architecture`: use `clean-architecture`. Existing global canonical skill at /Users/clairtonluz/.codex/skills/clean-architecture.
- `$luz-retry`: use `$luz-retry`. Canonical skill lives in resilience/luz-retry.
- `$luz-sidecar`: use `$luz-sidecar`. Canonical skill lives in cloud-native/luz-sidecar.

## DDD
Use DDD patterns when domain language, invariants, aggregate boundaries, and business rules are the problem.

- `$luz-aggregate`: Group domain objects into one consistency boundary.
- `$luz-aggregate-root`: Expose the only external entry point for modifying an aggregate.
- `$luz-application-service`: Orchestrate a use case, transaction, and boundary calls without owning domain rules.
- `$luz-domain-event`: Represent a meaningful domain fact that happened.
- `$luz-domain-factory`: Create domain objects or aggregates when construction is complex or invariant-heavy.
- `$luz-domain-service`: Place domain behavior that does not naturally belong to one entity or value object.
- `$luz-entity`: Model a domain concept with stable identity and lifecycle.
- `$luz-repository`: Provide collection-like access to aggregates while hiding persistence details.
- `$luz-specification`: Name and compose business predicates for validation, selection, or policy.
- `$luz-value-object`: Model an immutable concept identified by its values.

## Concurrency
Use concurrency patterns when scheduling, parallel work, synchronization, or capacity coordination is the problem.

- `$luz-active-object`: Expose asynchronous methods while executing work on the object's own scheduler.
- `$luz-leader-follower`: Coordinate worker threads so one waits for events while others are ready to take leadership.
- `$luz-producer-consumer`: Decouple work producers from consumers through a queue.
- `$luz-reactor`: Demultiplex events and dispatch them to non-blocking handlers.
- `$luz-read-write-lock`: Allow many concurrent readers or one exclusive writer.
- `$luz-thread-pool`: Reuse a bounded set of worker threads to execute tasks.

## Integration
Use integration patterns when systems exchange messages, events, or translated contracts.

- `$luz-content-based-router`: Route messages by inspecting validated message content.
- `$luz-dead-letter-queue`: Capture messages that cannot be processed successfully after defined attempts.
- `$luz-event-bus`: Provide a shared mechanism for publishing and subscribing to events.
- `$luz-message-broker`: Route, buffer, and deliver messages between producers and consumers.
- `$luz-message-queue`: Buffer messages so producers and consumers are decoupled in time.
- `$luz-message-translator`: Convert messages between different schemas or protocols at a boundary.
- `$luz-publish-subscribe`: Publish one event to multiple independent subscribers.
- `$luz-request-reply`: Send a request message and correlate the asynchronous reply.

## Cloud Native
Use cloud-native patterns when Kubernetes or platform infrastructure shapes runtime behavior.

- `$luz-ambassador`: Use a helper container or proxy to manage outbound communication for an application.
- `$luz-init-container`: Run setup containers to completion before starting the main application containers.
- `$luz-leader-election`: Elect one active instance among replicas to perform singleton work.
- `$luz-operator`: Automate operational knowledge with a controller that reconciles desired and actual state.
- `$luz-service-mesh`: Use platform-managed sidecar or node proxies for service-to-service traffic policy.
- `$luz-sidecar`: Run a helper container alongside an application container to provide supporting capabilities.

## Frontend
Use frontend patterns when UI composition, state flow, or presentation boundaries are the problem.

- `$luz-atomic-design`: Organize UI systems into atoms, molecules, organisms, templates, and pages.
- `$luz-component-pattern`: Build UI from focused, reusable components with explicit inputs and outputs.
- `$luz-container-presenter`: Separate data/state orchestration from presentational rendering.
- `$luz-flux`: Use one-way data flow with actions, dispatcher, stores, and views.
- `$luz-redux`: Manage client state through actions, pure reducers, and a single predictable store.

## Security
Use security patterns when identity, token handling, policy enforcement, or secure migration is the problem.

- `$luz-api-gateway-security`: Apply consistent edge security policies at the API gateway.
- `$luz-bff-token-handler`: Keep OAuth tokens in a backend-for-frontend and expose only session-specific responses to the browser.
- `$luz-oauth2-authorization-code`: Use the authorization code flow so a client obtains tokens through an authorization server after user consent or authentication.
- `$luz-strangler-security-migration`: Migrate legacy security behavior incrementally while preserving access control parity.
- `$luz-token-exchange`: Exchange one security token for another token with a narrower audience, scope, or delegation context.
- `$luz-zero-trust-architecture`: Design systems so every request is explicitly verified, least privileged, and monitored.

## Resilience
Use resilience patterns when failure containment, latency, overload, or degraded operation is the problem.

- `$luz-backpressure`: Make producers slow down or stop when consumers cannot keep up.
- `$luz-circuit-breaker`: Stop calling an unhealthy dependency temporarily so the system can fail fast and recover.
- `$luz-fallback`: Use an alternate response or path when the preferred path fails.
- `$luz-health-check`: Expose liveness, readiness, and dependency health signals.
- `$luz-load-shedding`: Intentionally reject lower-priority work when capacity is exhausted.
- `$luz-rate-limiting`: Restrict request rates to protect services and enforce fair use.
- `$luz-retry`: Repeat a failed operation under bounded, safe conditions.
- `$luz-timeout`: Bound how long work or external calls may wait.

## Data
Use data patterns when persistence mapping, transactions, identity, loading, or caching is the problem.

- `$luz-active-record`: Combine data and persistence operations on the same object.
- `$luz-cache-aside`: Let application code read from a cache and load/populate on misses.
- `$luz-dao`: Encapsulate low-level persistence operations for a data source.
- `$luz-data-mapper`: Move data between persistence structures and domain objects without coupling them.
- `$luz-identity-map`: Keep one in-memory object per identity within a scope.
- `$luz-lazy-loading`: Load data only when it is first needed.
- `$luz-unit-of-work`: Track changes in a business transaction and commit them as one unit.
- `$luz-write-through-cache`: Write data through the cache path so cache and store are updated together.
- `$luz-repository`: use `$luz-repository`. Canonical skill lives in ddd/luz-repository.

## Duplicate Pattern Policy
- Repository is canonical in `ddd/luz-repository`; data guidance references `$luz-repository`.
- Sidecar is canonical in `cloud-native/luz-sidecar`; architecture guidance references `$luz-sidecar`.
- Circuit Breaker and Retry are canonical in `resilience/luz-circuit-breaker` and `resilience/luz-retry`; architecture guidance references them.
- Clean Architecture is canonical in the existing global `clean-architecture` skill.
- API Gateway is canonical in `architecture/luz-api-gateway`; `$luz-api-gateway-security` covers edge security controls.
- Strangler Fig is canonical in `architecture/luz-strangler-fig`; `$luz-strangler-security-migration` covers security-specific migration.

## How To Use
1. Identify the problem category.
2. Open only the most specific skill for the pattern being considered.
3. Use `$luz-software-design-patterns-decision-guide` when the correct pattern is unclear.
4. Prefer direct code when the pattern would add names without reducing real complexity.
