# Engineering Standards Skills

This repository is the source of four Codex skills. It is not meant to be the repository where the skills are used.

The intended workflow is:
- download or clone this repository from GitHub
- copy the skill files into another repository
- use Codex from that target repository

## What This Repository Contains

- `AGENTS.md`
  Reference instructions showing how a repository can require the skill set.
- `.codex/skills/solid/SKILL.md`
  SOLID-focused design guidance.
- `.codex/skills/clean-code/SKILL.md`
  Readability and maintainability guidance.
- `.codex/skills/clean-architecture/SKILL.md`
  Architectural boundary guidance.
- `.codex/skills/secure-by-default/SKILL.md`
  Security-focused implementation guidance.

## What The Skills Do

The four skills are split by concern:

- `solid`
  Applies SRP, OCP, LSP, ISP, and DIP when code structure or abstractions are involved.
- `clean-code`
  Improves naming, cohesion, readability, local refactoring quality, and focused diffs.
- `clean-architecture`
  Keeps business rules in the correct layer and dependencies pointing inward.
- `secure-by-default`
  Preserves validation, authorization, least privilege, and safe data handling.

## Download From GitHub

Clone the repository:

```bash
git clone git@github.com:clairtonluz/engineering-standards.git
cd engineering-standards
```

Or download the ZIP from GitHub and extract it.

## How To Use These Skills In Another Repository

Copy these files from this repository into the target repository:

```text
AGENTS.md
.codex/skills/solid/SKILL.md
.codex/skills/clean-code/SKILL.md
.codex/skills/clean-architecture/SKILL.md
.codex/skills/secure-by-default/SKILL.md
```

Your target repository should end up with this structure:

```text
your-project/
├── AGENTS.md
└── .codex/
    └── skills/
        ├── clean-architecture/
        │   └── SKILL.md
        ├── clean-code/
        │   └── SKILL.md
        ├── secure-by-default/
        │   └── SKILL.md
        └── solid/
            └── SKILL.md
```

Then start Codex from the target repository. Codex will read that repository's `AGENTS.md` and use the four skills from `.codex/skills/`.

## If The Target Repository Already Has AGENTS.md

Do not overwrite it blindly.

Merge the instructions so the target repository keeps its existing repository-specific rules while also referencing these engineering-standard skills.

At minimum, the target repository should:
- list the four skills as available skills
- instruct Codex to use them together when appropriate, or always use them if that is the repository policy

## Example Usage In A Target Repository

After copying the files into another repository, run Codex there and ask for work such as:

- `review this service for clean architecture violations`
- `fix this bug with the smallest safe change`
- `refactor this use case to keep business logic out of the controller`
- `add tests for this behavior change`
- `write technical documentation following the engineering standards`

## When To Use These Skills

Use them in target repositories for:
- bug fixes
- feature work
- refactoring
- code review
- architecture review
- test updates
- technical documentation

It is especially useful when you want:
- strict layer ownership
- secure-by-default changes
- minimal, reviewable diffs
- consistent engineering standards across repositories

## Customizing The Skills

If you want to change the skill definitions themselves, edit:

- `.codex/skills/solid/SKILL.md`
- `.codex/skills/clean-code/SKILL.md`
- `.codex/skills/clean-architecture/SKILL.md`
- `.codex/skills/secure-by-default/SKILL.md`

If you want to change how another repository applies the skills, edit that repository's:

- `AGENTS.md`

The `SKILL.md` files define the standards. `AGENTS.md` defines how a repository tells Codex to use them.
