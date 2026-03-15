# Engineering Standards Skills

This repository contains four reusable engineering standards skills:

- `solid`
- `clean-code`
- `clean-architecture`
- `secure-by-default`

They are meant to be installed into Codex or referenced from Claude Code in another repository.

## What Is In This Repository

- `AGENTS.md`
  Reference repository instructions for Codex.
- `.codex/skills/solid/SKILL.md`
- `.codex/skills/clean-code/SKILL.md`
- `.codex/skills/clean-architecture/SKILL.md`
- `.codex/skills/secure-by-default/SKILL.md`

## Skill Overview

- `solid`
  Focuses on SRP, OCP, LSP, ISP, and DIP.
- `clean-code`
  Focuses on naming, readability, cohesion, and low-risk refactoring.
- `clean-architecture`
  Focuses on layer ownership and dependency direction.
- `secure-by-default`
  Focuses on validation, authorization, secrets, and safe defaults.

## Install On Codex

Codex uses skills from `~/.codex/skills/` for user-wide installation, or from `.codex/skills/` inside a specific project.

### Option 1: Install For Your User Profile

This makes the four skills available to all projects.

```bash
git clone https://github.com/clairtonluz/engineering-standards.git ~/.codex/engineering-standards
mkdir -p ~/.codex/skills
cp -R ~/.codex/engineering-standards/.codex/skills/* ~/.codex/skills/
```

After that, Codex can discover these skills from your user profile.

If you also want default instructions across projects, you can add guidance to your global Codex instructions separately. The skills themselves live in `~/.codex/skills/`.

### Option 2: Install In One Specific Project

This keeps the skills local to a single repository.

From inside the target repository:

```bash
mkdir -p .codex/skills
git clone https://github.com/clairtonluz/engineering-standards.git .tmp/engineering-standards
cp -R .tmp/engineering-standards/.codex/skills/* .codex/skills/
cp .tmp/engineering-standards/AGENTS.md ./AGENTS.md
```

Your project should then look like this:

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

If the target project already has an `AGENTS.md`, merge the relevant instructions instead of overwriting the file.

## Install On Claude Code

Claude Code does not use Codex skills as a native feature. The practical equivalent is to keep this repository somewhere on disk and reference the markdown guidance from `CLAUDE.md`.

### Option 1: Install For Your User Profile

This makes the standards available as global Claude Code guidance.

```bash
git clone https://github.com/clairtonluz/engineering-standards.git ~/.claude/engineering-standards
```

Then create or update `~/.claude/CLAUDE.md`:

```md
# Engineering Standards

Apply these engineering standards together when working on code:

- @~/.claude/engineering-standards/.codex/skills/solid/SKILL.md
- @~/.claude/engineering-standards/.codex/skills/clean-code/SKILL.md
- @~/.claude/engineering-standards/.codex/skills/clean-architecture/SKILL.md
- @~/.claude/engineering-standards/.codex/skills/secure-by-default/SKILL.md
```

### Option 2: Install In One Specific Project

This keeps the standards scoped to one repository.

From inside the target repository:

```bash
git clone https://github.com/clairtonluz/engineering-standards.git .tools/engineering-standards
```

Then create or update `CLAUDE.md` in the target project:

```md
# Project Instructions

Apply these engineering standards together:

- @.tools/engineering-standards/.codex/skills/solid/SKILL.md
- @.tools/engineering-standards/.codex/skills/clean-code/SKILL.md
- @.tools/engineering-standards/.codex/skills/clean-architecture/SKILL.md
- @.tools/engineering-standards/.codex/skills/secure-by-default/SKILL.md
```

If the project already has a `CLAUDE.md`, merge these references into the existing file.

## Typical Usage

After installation, ask for work such as:

- `review this service for clean architecture violations`
- `fix this bug with the smallest safe change`
- `refactor this use case to keep business logic out of the controller`
- `add tests for this behavior change`
- `check this flow for security risks`

## Updating

To update a cloned installation:

```bash
git -C ~/.codex/engineering-standards pull
cp -R ~/.codex/engineering-standards/.codex/skills/* ~/.codex/skills/
```

Or, if you installed for Claude Code:

```bash
git -C ~/.claude/engineering-standards pull
```

For project-local installs, pull inside the cloned project-local copy and re-copy files if needed.
