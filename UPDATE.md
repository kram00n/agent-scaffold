# UPDATE.md

Instructions for an AI coding agent updating this scaffold in a repository that already has it installed. Give this file to your agent — paste its contents, or point it at the raw URL — and ask it to update. It's plain markdown, so you can also follow it by hand.

## Steps

1. **Confirm the scaffold is already installed.** Check for `.agent-scaffold/` and a root `AGENTS.md`. If either is missing, stop — this repo needs [SETUP.md](./SETUP.md), not this file.

2. **Update the submodule:**
   ```bash
   git submodule update --remote .agent-scaffold
   ```
   If this reports no change, the scaffold is already current — tell the user that and stop.

3. **Check which path this repo uses**, by looking at the root `AGENTS.md`:
   - If it contains the line `@.agent-scaffold/AGENTS.md`, this is the **Claude Code import path** — nothing else to do. The import resolves fresh from `.agent-scaffold/AGENTS.md` every session, so the update is already complete.
   - Otherwise, this is the **copy path** — continue to step 4.

4. **(Copy path only) Refresh the baseline, preserving project-specific additions.** The baseline is everything above the `## Project-specific additions` heading in root `AGENTS.md`; everything from that heading down is the project's own content and must be left untouched.
   - Diff the old baseline (currently above that heading in `AGENTS.md`) against the new `.agent-scaffold/AGENTS.md`, and show the user what changed.
   - Replace the content above `## Project-specific additions` with the new `.agent-scaffold/AGENTS.md` content.
   - If root `AGENTS.md` doesn't have a `## Project-specific additions` heading (renamed, removed, or never used), stop and ask the user how to merge instead of guessing — don't overwrite their edits blindly.

5. **Stage the changes, then stop and confirm before committing:**
   ```bash
   git add .agent-scaffold AGENTS.md
   ```
   Show the user the proposed [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) message (e.g. `chore: update agent-scaffold baseline`) and the list of staged files. Get explicit confirmation before running `git commit`.

6. **Don't push without asking.** Confirm with the user before running `git push`.

7. **Report back**: summarize what changed in the baseline — don't just say "updated" — and confirm whether any project-specific content was touched.
