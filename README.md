# aiscaffold

Baseline `AGENTS.md` for AI coding agents (Claude Code, Codex, and others), meant to be dropped into every new project as a git submodule.

Sections 2-5 are adapted from [Andrej Karpathy's CLAUDE.md guidelines](https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md). Section 1 (Docker-only workflow) is this repo's own addition.

## Usage

Agent tools look for `AGENTS.md` at a project's root, but a submodule lives in a subdirectory — so add the submodule and symlink the file to the root:

```bash
git submodule add https://github.com/<you>/aiscaffold.git .agent-scaffold
ln -s .agent-scaffold/AGENTS.md AGENTS.md
git add .gitmodules .agent-scaffold AGENTS.md
git commit -m "Add aiscaffold AGENTS.md"
```

## Updating

```bash
git submodule update --remote .agent-scaffold
```

## Adding project-specific rules

`AGENTS.md` is a symlink into the shared submodule, so it can't be edited in place without editing the shared file itself. If a project needs its own rules on top of the baseline, break the symlink and edit a real copy:

```bash
rm AGENTS.md
cp .agent-scaffold/AGENTS.md AGENTS.md
```

After that, `AGENTS.md` is a normal file you can extend freely — but future updates to this scaffold will need to be re-merged in by hand instead of picked up automatically via `git submodule update`.
