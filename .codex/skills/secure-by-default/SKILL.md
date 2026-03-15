---
name: secure-by-default
description: Apply secure-by-default engineering practices for validation, authorization, data handling, and safe operational behavior.
---

# Secure By Default Skill

## Purpose

Use this skill when the task touches external input, authentication, authorization, secrets, logging, file handling, network access, serialization, or other security-sensitive behavior.

This skill enforces:
- validation at boundaries
- least privilege
- preservation of authorization checks
- safe handling of secrets and sensitive data
- constrained file, serialization, and network behavior

## Core Rules

### Validate External Input

Validate untrusted input at system boundaries.

Prefer:
- explicit schema or contract validation
- narrow accepted formats
- rejecting malformed or unexpected values early

### Preserve Authentication And Authorization

Do not weaken:
- authn checks
- authz checks
- tenant scoping
- user scoping
- ownership checks

If a change affects access rules, verify them intentionally.

### Protect Sensitive Data

Never:
- hardcode credentials or tokens
- log secrets
- log passwords
- log personal or sensitive data without a justified need

Expose only the minimum data required by the use case.

### Safe Error Handling

Provide enough detail for operators and maintainers without leaking sensitive internals to external consumers.

### Safe Defaults For Integrations

Keep deserialization, file handling, and network access constrained.

Avoid:
- permissive parsing without validation
- unsafe filesystem paths
- unbounded external requests
- insecure cryptography choices

### Least Privilege

Prefer the smallest required access and capabilities for each component and operation.

## Decision Heuristics

Ask:
1. Is this input trusted?
2. What authorization rule protects this operation?
3. Could this log or response expose sensitive data?
4. Is the external interaction constrained enough?
5. What is the safe default if input is invalid or access is denied?

## Review Checklist

Verify that:
- boundary validation exists
- authn and authz still hold
- tenant and user scope are preserved
- logs do not leak secrets or personal data
- secrets are not embedded in code
- external calls and file handling are constrained
- error responses do not reveal sensitive internals

## Output Expectations

When using this skill:
1. explain the security risk or invariant involved
2. preserve or strengthen the security boundary
3. implement the smallest safe change
4. add or update tests when behavior changes
5. mention any usability vs security trade-off
