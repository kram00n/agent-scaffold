# agent-scaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Setup

Give [`SETUP.md`](./SETUP.md) to your coding agent — paste its contents, or point it at the raw URL — and ask it to install this scaffold into your repository. It'll add the submodule, pick the right `AGENTS.md`/`CLAUDE.md` wiring for the tools you use (Claude Code alone, or Codex/Cursor/a mix), and commit the result.

Copy-paste prompt:

```
Fetch https://raw.githubusercontent.com/Kramoon/agent-scaffold/main/SETUP.md and follow its instructions to install the agent-scaffold baseline into this repository.
```

Prefer to do it by hand? `SETUP.md` is plain markdown with the exact commands — open it and follow along yourself.

## Updating

Give [`UPDATE.md`](./UPDATE.md) to your coding agent and ask it to update the scaffold. It pulls the latest submodule commit and, if you're on the copy path, refreshes the baseline while preserving your project-specific additions.

Copy-paste prompt:

```
Fetch https://raw.githubusercontent.com/Kramoon/agent-scaffold/main/UPDATE.md and follow its instructions to update the agent-scaffold baseline in this repository.
```

Prefer to do it by hand? `git submodule update --remote .agent-scaffold` pulls the latest commit. If you're on the Claude-only import path, that's the whole update. If you're on the copy path, also refresh the content above the `## Project-specific additions` heading in `AGENTS.md` with the new `.agent-scaffold/AGENTS.md`, and reapply any edits that got overwritten.

## Adding project-specific rules

Add rules directly to your project's root `AGENTS.md`, below the baseline — whether that's the `@import` line or the copied text. The scaffold in `.agent-scaffold/` stays untouched either way.

## License

[MIT](./LICENSE)
