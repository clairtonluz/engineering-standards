# Engineering Standards Skill

This repository is the source of a Codex skill. It is not meant to be the repository where the skill is used.

The intended workflow is:
- download or clone this repository from GitHub
- copy the skill files into another repository
- use Codex from that target repository

## What This Repository Contains

- `AGENTS.md`
  Reference instructions showing how a repository can require the `engineering-standards` skill.
- `.codex/skills/engineering-standards/SKILL.md`
  The skill definition with the actual engineering rules.

## What The Skill Does

The skill guides Codex to:
- apply SOLID, Clean Code, and Clean Architecture principles
- make the smallest safe change
- keep business rules in the correct layer
- preserve validation, authorization, and sensitive-data protections
- add or update relevant tests for behavior changes
- finish with a concise review summary covering root cause, risks, tests, and trade-offs

## Download From GitHub

Clone the repository:

```bash
git clone git@github.com:clairtonluz/engineering-standards.git
cd engineering-standards
```

Or download the ZIP from GitHub and extract it.

## How To Use This Skill In Another Repository

Copy these files from this repository into the target repository:

```text
AGENTS.md
.codex/skills/engineering-standards/SKILL.md
```

Your target repository should end up with this structure:

```text
your-project/
├── AGENTS.md
└── .codex/
    └── skills/
        └── engineering-standards/
            └── SKILL.md
```

Then start Codex from the target repository. Codex will read that repository's `AGENTS.md` and use the skill from `.codex/skills/engineering-standards/SKILL.md`.

## If The Target Repository Already Has AGENTS.md

Do not overwrite it blindly.

Merge the instructions so the target repository keeps its existing repository-specific rules while also referencing the `engineering-standards` skill.

At minimum, the target repository should:
- list `engineering-standards` as an available skill
- instruct Codex to use it when appropriate, or always use it if that is the repository policy

## Example Usage In A Target Repository

After copying the files into another repository, run Codex there and ask for work such as:

- `review this service for clean architecture violations`
- `fix this bug with the smallest safe change`
- `refactor this use case to keep business logic out of the controller`
- `add tests for this behavior change`
- `write technical documentation following the engineering standards`

## When To Use This Skill

Use it in target repositories for:
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

## Customizing The Skill

If you want to change the skill itself, edit:

- `.codex/skills/engineering-standards/SKILL.md`

If you want to change how another repository applies the skill, edit that repository's:

- `AGENTS.md`

`SKILL.md` defines the standards. `AGENTS.md` defines how a repository tells Codex to use them.
