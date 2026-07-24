# agent-scaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Setup

Give [`SETUP.md`](./SETUP.md) to your coding agent — paste its contents, or point it at the raw URL — and ask it to install this scaffold into your repository. It'll add the submodule, pick the right `AGENTS.md`/`CLAUDE.md` wiring for the tools you use (Claude Code alone, or Codex/Cursor/a mix), and commit the result.

Prefer to do it by hand? `SETUP.md` is plain markdown with the exact commands — open it and follow along yourself.

## Updating

```bash
git submodule update --remote .agent-scaffold
```

If you're on the Claude-only import path, that's the whole update. If you're on the copy path, also re-run `cp .agent-scaffold/AGENTS.md AGENTS.md` and reapply any project-specific edits that got overwritten.

## Adding project-specific rules

Add rules directly to your project's root `AGENTS.md`, below the baseline — whether that's the `@import` line or the copied text. The scaffold in `.agent-scaffold/` stays untouched either way.
