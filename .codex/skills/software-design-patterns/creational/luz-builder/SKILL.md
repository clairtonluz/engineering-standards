---
name: luz-builder
description: Apply the Builder design pattern. Use when objects have many optional values or creation steps. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Builder

## What It Is
Construct complex objects step by step while preserving validation and readability.

## When To Use It
- Objects have many optional values or creation steps.
- Construction needs validation before use.
- The final object should be immutable or hard to misuse.

## When Not To Use It
- A constructor or static factory is clear.
- The builder allows invalid partial objects to escape.
- The builder only mirrors every field without adding clarity.

## Main Benefits
- Clarifies ownership and intent when the problem genuinely matches the pattern.
- Improves testability by giving collaborators explicit roles.
- Reduces duplication when repeated decisions are moved into one clear structure.

## Trade-Offs
- Adds names and indirection that must earn their keep.
- Can make debugging harder if responsibilities are split without a clear boundary.
- May be unnecessary when a direct function, class, or module is enough.

## Common Mistakes
- Applying the pattern because it is familiar instead of because the problem needs it.
- Letting the pattern hide business rules, validation, authorization, or failure handling.
- Creating abstractions before there are at least two real variations or a clear boundary.

## Implementation Checklist
- Name the problem this pattern is solving.
- Keep the public API small and explicit.
- Keep business rules in the owning layer, not in framework glue.
- Prefer constructor injection over hidden globals.
- Write tests around behavior and boundaries, not internal class structure.
- Document the trade-off if the pattern adds visible indirection.

## Example Implementation
```java
public record SearchQuery(String text, int page, int size, Set<String> tags) {
    public static Builder builder(String text) {
        return new Builder(text);
    }

    public static final class Builder {
        private final String text;
        private int page = 0;
        private int size = 20;
        private final Set<String> tags = new LinkedHashSet<>();

        private Builder(String text) {
            this.text = Objects.requireNonNull(text);
        }

        public Builder page(int page) {
            if (page < 0) throw new IllegalArgumentException("page must be non-negative");
            this.page = page;
            return this;
        }

        public Builder tag(String tag) {
            this.tags.add(Objects.requireNonNull(tag));
            return this;
        }

        public SearchQuery build() {
            return new SearchQuery(text, page, size, Set.copyOf(tags));
        }
    }
}
```
Validate in the builder so invalid objects cannot be created.

## Testing Strategy
- Unit test the behavior that the pattern is meant to protect.
- Add boundary tests for invalid inputs, failure paths, and edge cases.
- Prefer fakes or in-memory adapters over mocking implementation details.
- Add integration tests when the pattern crosses a framework, persistence, network, or deployment boundary.

## Security Considerations
Validate inputs at the pattern boundary and avoid hiding authorization, identity, or data-handling decisions behind generic abstractions.

## Observability Considerations
Expose useful logs, metrics, or traces at the boundary where this pattern changes control flow, data flow, or external calls.

## Platform Notes
- Java/Spring Boot: Use dependency injection, small interfaces, and focused unit tests instead of static global access.
- TypeScript/Frontend: Use typed interfaces, dependency injection where practical, and accessible UI states when the pattern affects presentation.
- Kubernetes/Cloud: Keep platform concerns in configuration, infrastructure adapters, or deployment manifests rather than leaking them into domain logic.

## Choosing Between Related Patterns
- Use `$luz-factory-method` when that pattern better matches the problem.
- Use `$luz-abstract-factory` when that pattern better matches the problem.
- Use `$luz-domain-factory` when that pattern better matches the problem.
