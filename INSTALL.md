# Install

This repository is designed for the shared `skills` CLI.

Source repository:

- `BakerSean168/agent-skills`

## Quickstart

Install interactively:

```bash
npx skills@latest add BakerSean168/agent-skills
```

This lets you:

- choose which skills to install
- choose which coding agents to install them for
- install at project scope by default

## Verified discovery

The repository has been smoke-tested with:

```bash
npx skills@latest add BakerSean168/agent-skills --list
```

Historical smoke test (before `personal-agent-onboarding` was added):

- the CLI successfully cloned the repository
- it discovered `coding-coach` and `skill-scaffold`

The catalog now also includes `personal-agent-onboarding`. Run the above `--list` command to verify current discovery on your machine.

## Project install for Codex

Install one skill into the current project:

```bash
npx skills@latest add BakerSean168/agent-skills \
  --skill coding-coach \
  --agent codex \
  -y \
  --copy
```

Verified project-level output:

```text
.agents/
  skills/
    coding-coach/
skills-lock.json
```

Use `--skill skill-scaffold` for the scaffold skill or `--skill personal-agent-onboarding` for trusted Personal Agent initialization.

## Project install for Claude Code

Install one skill into the current project:

```bash
npx skills@latest add BakerSean168/agent-skills \
  --skill coding-coach \
  --agent claude-code \
  -y \
  --copy
```

Verified project-level output:

```text
.claude/
  skills/
    coding-coach/
skills-lock.json
```

## Install all skills

Install all skills for a chosen agent:

```bash
npx skills@latest add BakerSean168/agent-skills \
  --skill '*' \
  --agent codex \
  -y \
  --copy
```

Or let the CLI prompt you interactively:

```bash
npx skills@latest add BakerSean168/agent-skills
```

## Global install

Use `-g` for a user-level install managed by the `skills` CLI:

```bash
npx skills@latest add BakerSean168/agent-skills -g
```

You can combine it with `--agent` and `--skill`:

```bash
npx skills@latest add BakerSean168/agent-skills \
  -g \
  --agent claude-code \
  --skill coding-coach \
  -y
```

## Manual fallback

If you do not want to use the `skills` CLI, the repository source of truth is the root `skills/` directory:

```text
skills/
  coding-coach/
  skill-scaffold/
  personal-agent-onboarding/
```

For project-local usage, the verified fallback locations are:

- Codex: copy a skill folder into `.agents/skills/<skill-name>/`
- Claude Code: copy a skill folder into `.claude/skills/<skill-name>/`

Example:

```text
skills/coding-coach -> .agents/skills/coding-coach
skills/coding-coach -> .claude/skills/coding-coach
```

## Notes

- The repository follows the standard `skills/` catalog layout expected by the shared installer ecosystem.
- `.claude-plugin/marketplace.json` is included so plugin-aware tooling can discover the catalog.
- During the Claude Code smoke test, the CLI banner still printed `codex agent detected` because the command was run from a Codex environment, but the actual install path correctly targeted `.claude/skills/`.
