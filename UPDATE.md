# UPDATE.md

Instructions for an AI coding agent updating this scaffold in a repository that already has it installed. An agent can follow these steps automatically when the check described in `AGENTS.md` is due, or a user can request an immediate update.

## Steps

1. **Check the installation and local changes.** Confirm that `.agent-scaffold/` is an initialized submodule and that the project has a root `AGENTS.md`. If not, skip the automatic update and tell the user; for a manual update, stop and use [SETUP.md](./SETUP.md) if the scaffold is not installed. Check `git status --porcelain -- .agent-scaffold AGENTS.md` and the submodule's own `git status --porcelain`. If either reports changes, skip the update and tell the user so their work is not overwritten. Do not mark a skipped check as successful.

2. **Check the project wiring before updating.** If root `AGENTS.md` contains the line `@.agent-scaffold/AGENTS.md`, this is the Claude Code import path. Otherwise, it is the copy path: require the exact `## Project-specific additions` heading and compare the content above it with the current `.agent-scaffold/AGENTS.md`, allowing only separator whitespace. If the heading is missing or the copied baseline contains local edits, skip the update and tell the user that the baseline needs a manual merge. Preserve everything from the heading onward exactly.

3. **Update the submodule.** Record its current commit, then run:
   ```bash
   git submodule update --remote .agent-scaffold
   ```
   If the command fails, tell the user and do not mark the check as successful. For an automatic check, continue the requested project work when possible. Compare the resulting submodule commit with the recorded commit; if they match, the scaffold was already current.

4. **Refresh the copy if needed.** If the commit changed and the project uses the copy path, replace the content above `## Project-specific additions` in root `AGENTS.md` with the new `.agent-scaffold/AGENTS.md`, leaving the heading and everything below it untouched. For the import path, no copy is needed.

5. **Record and report the check.** After a successful check, including one that found no update, write the current Unix timestamp to the file named by `git rev-parse --git-path agent-scaffold-last-check`. If the commit changed, summarize the baseline changes and tell the user that the updated instructions apply in a new agent session.

6. **Leave automatic updates for review.** Do not stage, commit, or push an automatic update. Continue the requested work and report the changed files. For a manual update with changes, stage `.agent-scaffold` and, for the copy path, `AGENTS.md`. Show the user the proposed [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) message (e.g. `chore: update agent-scaffold baseline`), including the agent attribution trailer, and the staged files. If the requested workflow includes a push, show its destination too. Get explicit confirmation before committing; that confirmation also authorizes pushing that commit to the stated destination without a second confirmation.
