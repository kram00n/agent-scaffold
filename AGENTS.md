# AGENTS.md

Baseline instructions for AI coding agents (Claude Code, Codex, and others) working in this repository. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Docker Only — No Local Anything

**Everything runs inside Docker. There is no local install or execution path.**

- No local installs. Never run `npm install`, `pip install`, `bundle install`, or similar on the host. Dependencies are installed inside the image via `Dockerfile` / `docker-compose.yml`.
- No local execution. Never run the app, a script, a linter, or a REPL directly on the host. Everything runs via `docker compose run`, `docker compose exec`, or `docker run`.
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

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
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
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 5. Goal-Driven Execution

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

## 6. Conventional Commits

**Every commit message follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).**

- Format: `<type>[optional scope]: <description>`, e.g. `fix(auth): handle expired refresh tokens`.
- Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `build`, `ci`, `perf`.
- Breaking changes: append `!` after the type/scope, or add a `BREAKING CHANGE:` footer.
- Description is imperative mood, lowercase, no trailing period.

---

**These guidelines are working if:** nothing runs on the host, fewer unnecessary changes show up in diffs, fewer rewrites happen due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
