# AGENTS.md

Baseline instructions for AI coding agents (Claude Code, Codex, and others) working in this repository. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Docker Only — Project Code Never Runs on the Host

**The project's own code, dependencies, and tooling never touch the host. Only they run inside Docker.**

This applies to the project itself — its dependencies, its app code, its tests, its linters. It does not apply to host orchestration tools like `git`, `gh`, or the `docker`/`docker compose` CLI itself — those are expected to run directly.

- No local installs. Never run `npm install`, `pip install`, `bundle install`, or similar on the host. Dependencies are installed inside the image via `Dockerfile` / `docker-compose.yml`.
- No local execution of project code. Never run the app, a project script, a linter, or a REPL directly on the host. Everything runs via `docker compose run`, `docker compose exec`, or `docker run`.
- No local testing. Tests run inside a container (e.g. `docker compose run --rm app <test command>`), never with a host-installed test runner.
- If no `Dockerfile` / `docker-compose.yml` exists yet, create one first. Don't work around a missing Docker setup by falling back to local commands — fix the Docker setup instead.
- Use the project's existing compose services and commands rather than inventing new invocation patterns. Ask if the right service or command is unclear.
- If a task genuinely can't be done through Docker, stop and say so rather than quietly running it on the host.

## 2. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 3. Simplicity First

**The simplest implementation that fully meets the current requirements. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No indirection that doesn't earn its place — a layer you added "in case" is a layer to delete.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 4. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Remove the paths YOUR changes made obsolete. Don't leave a compatibility layer, fallback, or migration shim behind to keep the old path alive — replace it.
- Don't remove pre-existing dead code unless asked. Mention it instead.

The boundary: obsolete because of your change → delete it. Obsolete before you got there → mention it.

If the old path has external consumers (a published API, a stored format, another service), say so and ask before removing it.

The test: Every changed line should trace directly to the user's request.

## 5. Grow the System in Layers

**Always have something that works. Add capability on top of it.**

- Start from the smallest version that works end to end, then add each new capability on top of a product that already works.
- Never trade a working product for unfinished complexity. A half-migrated system is worse than the one you started with.
- If a change can't land in one working step, sequence it into steps that each leave the system working — and say what the steps are before starting.
- Keep components modular and concerns clearly separated, so the next layer has somewhere to attach.

**On structure vs. simplicity:** Rule 3 governs how much you build; this rule governs how you shape it. Build the simplest thing that meets the current requirement, but make the structural decisions — module boundaries, data models, interfaces — ones you'd be willing to keep. Don't reach for a stopgap that only works for now and is meant to be replaced later; if a real stopgap is genuinely the right call, name it as one and say what would replace it.

## 6. Use What's Already There

**Check before you build. Check before you add.**

- Lean on the dependencies already in the project before writing your own implementation or adding a package.
- Don't assume a library lacks a capability without checking its documentation and types. Look it up.
- Prefer established, well-maintained libraries when they reduce overall complexity or improve reliability. Don't reimplement common functionality without a clear reason.
- Adding a dependency is a decision with a cost. Say why the existing options don't cover it before pulling one in.

## 7. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

All verification happens inside Docker — see Rule 1.

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 8. Conventional Commits

**Every commit message follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).**

- Format: `<type>[optional scope]: <description>`, e.g. `fix(auth): handle expired refresh tokens`.
- Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `build`, `ci`, `perf`.
- Breaking changes: append `!` after the type/scope, or add a `BREAKING CHANGE:` footer.
- Description is imperative mood, lowercase, no trailing period.
- Before committing, stop and show the user the proposed commit message and the list of files staged. Get explicit confirmation before running `git commit`.
- Before pushing, stop and get explicit confirmation from the user. Never run `git push` unprompted.

### Agent attribution

**Every commit an AI agent helped produce carries a `Co-Authored-By:` trailer naming that agent.**

- Format: `Co-Authored-By: <agent name> <email>`, as the last lines of the commit message, separated from the body by a blank line.
- Use the specific model name, not a generic label — `Claude Sonnet 5`, not `Claude` or `AI`. If the model has a distinguishing variant, include it as the model reports it (e.g. `Claude Opus 5 (1M context)`).
- Anthropic models use `<noreply@anthropic.com>`. For other agents, use that vendor's documented no-reply address.
- If several agents contributed, add one trailer per agent, in the order they contributed.
- This applies to any commit the agent had a hand in — writing the code, editing it, or generating the message. It is not limited to commits the agent ran `git commit` for. The human driving the work stays the commit author; the agent is a co-author.
- Show these trailers as part of the proposed commit message when confirming with the user.

```
feat(auth): add refresh token rotation

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
```

---

**These guidelines are working if:** project code never runs outside Docker, fewer unnecessary changes show up in diffs, fewer rewrites happen due to overcomplication, the system works at the end of every step rather than only at the end of the task, dependencies get reused instead of reinvented, agent involvement is visible in the commit history, and clarifying questions come before implementation rather than after mistakes.
