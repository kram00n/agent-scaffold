# agent-scaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-4 and 7 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Sections 5-6 are adapted from [an AGENTS.md list shared by Marcos Hernanz](https://x.com/MarcosHernanz/status/2083954734487212511). Section 1 (Docker-only workflow) and Section 8 (Conventional Commits and agent attribution) are this repo's own additions.

## Setup

Give [`SETUP.md`](./SETUP.md) to your coding agent — paste its contents, or point it at the raw URL — and ask it to install this scaffold into your repository. It'll add the submodule, pick the right `AGENTS.md`/`CLAUDE.md` wiring for the tools you use (Claude Code alone, or Codex/Cursor/a mix), and stage the result. It will show you the proposed commit for confirmation; if the requested workflow includes a push, the same confirmation covers the stated destination.

Copy-paste prompt:

```
Fetch https://raw.githubusercontent.com/kram00n/agent-scaffold/main/SETUP.md and follow its instructions to install the agent-scaffold baseline into this repository.
```

Prefer to do it by hand? `SETUP.md` is plain markdown with the exact commands — open it and follow along yourself.

## Updating

When an agent starts work in a project using this scaffold, it checks for an update if the last successful check was at least seven days ago in that checkout. If an update is available, it updates the submodule and refreshes copied baseline instructions while preserving project-specific additions. Updates are left uncommitted for review and take effect in a new agent session. This happens only when the project is used; there is no background scheduler. Existing installations need the manual update below once to receive these instructions.

You can also give [`UPDATE.md`](./UPDATE.md) to your coding agent and ask it to update the scaffold immediately.

Copy-paste prompt:

```
Fetch https://raw.githubusercontent.com/kram00n/agent-scaffold/main/UPDATE.md and follow its instructions to update the agent-scaffold baseline in this repository.
```

Prefer to do it by hand? Follow the checks in `UPDATE.md`, then run `git submodule update --remote .agent-scaffold`. If you're on the Claude-only import path, that's the whole update. If you're on the copy path, also refresh the content above the `## Project-specific additions` heading in `AGENTS.md` with the new `.agent-scaffold/AGENTS.md`, preserving your project-specific additions.

## Adding project-specific rules

Add rules directly to your project's root `AGENTS.md`, below the baseline — whether that's the `@import` line or the copied text. The scaffold in `.agent-scaffold/` stays untouched either way.

If you're on the copy path, keep the `## Project-specific additions` heading intact: [`UPDATE.md`](./UPDATE.md) looks for that exact heading to know where the baseline ends and your own content starts. Renaming or removing it doesn't break anything immediately, but it stops the automated merge from finding the boundary on the next update.

## License

[MIT](./LICENSE)
