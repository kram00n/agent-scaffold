# agent-scaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Setup

Agent tools look for `AGENTS.md` at a project's root, but a submodule lives in a subdirectory. Start by adding the submodule:

```bash
git submodule add https://github.com/Kramoon/agent-scaffold.git .agent-scaffold
```

Claude Code only reads `CLAUDE.md`, not `AGENTS.md` ([per its docs](https://code.claude.com/docs/en/memory#agentsmd)), and it's the only tool that understands `@path` imports — other tools like Codex or Cursor read `AGENTS.md` directly and treat `@` as plain text. Pick the path that matches what you use:

### Claude Code only

Import the baseline so Claude expands it automatically at session start, and symlink `CLAUDE.md` to pick up the same chain:

```markdown
# AGENTS.md

@.agent-scaffold/AGENTS.md

## Project-specific additions
- ...
```

```bash
ln -s AGENTS.md CLAUDE.md
```

(On Windows, symlinks need Administrator privileges or Developer Mode. Use a real `CLAUDE.md` containing `@AGENTS.md` instead if that's a problem.)

Updates propagate automatically — `git submodule update --remote .agent-scaffold` is all you need; there's nothing else to re-run, since the import resolves fresh every session.

### Codex, Cursor, or a mix of agents

Non-Claude tools don't expand `@` imports, so give them the actual text instead of a reference:

```bash
cp .agent-scaffold/AGENTS.md AGENTS.md
ln -s AGENTS.md CLAUDE.md   # so Claude Code reads the same file too
```

This works for every tool that reads `AGENTS.md` as plain text, Claude Code included. The tradeoff: it's a copy, not a live reference, so re-run the `cp` (and reapply any project-specific edits) after updating the submodule — nothing refreshes on its own.

```bash
git add .gitmodules .agent-scaffold AGENTS.md CLAUDE.md
git commit -m "feat: add agent-scaffold AGENTS.md"
```

## Updating

```bash
git submodule update --remote .agent-scaffold
```

If you're on the Claude-only import path, that's the whole update. If you're on the copy path, also re-run `cp .agent-scaffold/AGENTS.md AGENTS.md` and reapply any project-specific edits that got overwritten.

## Adding project-specific rules

Add rules directly to your project's root `AGENTS.md`, below the baseline — whether that's the `@import` line or the copied text. The scaffold in `.agent-scaffold/` stays untouched either way.
