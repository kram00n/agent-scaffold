# aiscaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Usage

Agent tools look for `AGENTS.md` at a project's root, but a submodule lives in a subdirectory. Add the submodule, then create a root `AGENTS.md` that points at it:

```bash
git submodule add https://github.com/<you>/aiscaffold.git .agent-scaffold
```

```markdown
# AGENTS.md

Baseline rules: see `.agent-scaffold/AGENTS.md` — read and follow it first.

## Project-specific additions
- ...
```

```bash
git add .gitmodules .agent-scaffold AGENTS.md
git commit -m "feat: add aiscaffold AGENTS.md"
```

## Updating

```bash
git submodule update --remote .agent-scaffold
```

## Adding project-specific rules

The root `AGENTS.md` is a real file, not a symlink — it just points at the shared baseline. Add project-specific rules directly to it, under the reference line. The baseline in `.agent-scaffold/` stays untouched and keeps updating independently via `git submodule update --remote`, so there's nothing to re-merge by hand.
