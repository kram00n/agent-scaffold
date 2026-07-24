# SETUP.md

Instructions for an AI coding agent installing this scaffold into a repository. Give this file to your agent — paste its contents, or point it at the raw URL — and ask it to set things up. It's plain markdown, so you can also just follow it by hand.

## Steps

1. **Check for conflicts first.** If the repo already has an `AGENTS.md` or `CLAUDE.md`, stop and ask the user before overwriting either — don't clobber existing instructions. Also check whether `.agent-scaffold/` already exists; if it does and isn't already this submodule, stop and ask the user how to proceed instead of letting `git submodule add` fail on it.

2. **Add the submodule:**
   ```bash
   git submodule add https://github.com/Kramoon/agent-scaffold.git .agent-scaffold
   ```

3. **Ask which agents this project uses** (or infer it from what's already configured), then follow the matching path:

   **Claude Code only** — import the baseline so it's always current, and symlink `CLAUDE.md` since Claude Code reads `CLAUDE.md`, not `AGENTS.md`:
   ```markdown
   # AGENTS.md

   @.agent-scaffold/AGENTS.md

   ## Project-specific additions
   - ...
   ```
   ```bash
   ln -s AGENTS.md CLAUDE.md
   ```
   (Windows: symlinks need Administrator privileges or Developer Mode — use a real `CLAUDE.md` containing `@AGENTS.md` instead.)

   **Codex, Cursor, or a mix of agents** — copy the baseline instead, since non-Claude tools don't expand `@` imports. Append the `## Project-specific additions` heading yourself — a plain `cp` won't include it, and [`UPDATE.md`](./UPDATE.md) needs that exact heading later to know where the baseline ends and project-specific content begins:
   ```bash
   cp .agent-scaffold/AGENTS.md AGENTS.md
   printf '\n## Project-specific additions\n- ...\n' >> AGENTS.md
   ln -s AGENTS.md CLAUDE.md
   ```
   Tell the user this copy won't auto-update: after `git submodule update --remote .agent-scaffold`, the `cp` needs to be re-run and any project-specific edits reapplied.

4. **Stage the files, then stop and confirm before committing:**
   ```bash
   git add .gitmodules .agent-scaffold AGENTS.md CLAUDE.md
   ```
   Show the user the proposed [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) message (e.g. `feat: add agent-scaffold AGENTS.md`) and the list of staged files. Get explicit confirmation before running `git commit`.

5. **Don't push without asking.** Setup changes repo state and history — confirm with the user before running `git push`.

6. **Report back**: which path you chose and why, and point the user at `.agent-scaffold/README.md` (the scaffold's own README, now present in the repo) for how to update the scaffold later or add further project-specific rules.
