# agent-scaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Usage

Agent tools look for `AGENTS.md` at a project's root, but a submodule lives in a subdirectory. Add the submodule, then create a root `AGENTS.md` that imports it:

```bash
git submodule add https://github.com/Kramoon/agent-scaffold.git .agent-scaffold
```

```markdown
# AGENTS.md

@.agent-scaffold/AGENTS.md

## Project-specific additions
- ...
```

The `@path` syntax is a [Claude Code import](https://code.claude.com/docs/en/memory#import-additional-files) — Claude expands it in place at session start, so the baseline is loaded automatically, not just when the agent happens to follow a "go read this" instruction. Tools that don't recognize `@` imports will see it as plain text; open `.agent-scaffold/AGENTS.md` manually if yours doesn't auto-expand.

Claude Code specifically reads `CLAUDE.md`, not `AGENTS.md` ([per its docs](https://code.claude.com/docs/en/memory#agentsmd)) — add a symlink so it picks up the same chain:

```bash
ln -s AGENTS.md CLAUDE.md
```

(On Windows, symlinks need Administrator privileges or Developer Mode. Use a real `CLAUDE.md` containing `@AGENTS.md` instead if that's a problem.)

```bash
git add .gitmodules .agent-scaffold AGENTS.md CLAUDE.md
git commit -m "feat: add agent-scaffold AGENTS.md"
```

## Updating

```bash
git submodule update --remote .agent-scaffold
```

## Adding project-specific rules

The root `AGENTS.md` is a real file, not a symlink — it just points at the shared baseline. Add project-specific rules directly to it, under the reference line. The baseline in `.agent-scaffold/` stays untouched and keeps updating independently via `git submodule update --remote`, so there's nothing to re-merge by hand.
