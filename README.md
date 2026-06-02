# Aluvia Skills

[![skills.sh](https://skills.sh/b/aluvia-connect/skills)](https://skills.sh/aluvia-connect/skills)

Agent skills for [Aluvia](https://aluvia.io) and related workflows. Format follows the [Agent Skills](https://agentskills.io) standard for Cursor, Claude Code, OpenClaw, and other compatible hosts.

No application code or `package.json` in this repo — documentation and `SKILL.md` files only.

## What's in this repo

| Path                 | Purpose                                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| `aluvia/`            | Unblock web requests via mobile carrier proxies — CLI sessions, CDP, block bypass               |
| `webtomd/`           | Scrape URLs to clean Markdown (fast or precise); uses Aluvia proxy when `ALUVIA_API_KEY` is set |
| `docs/`              | Aluvia CLI reference: command reference, workflows, troubleshooting                             |
| `docs/integrations/` | Tool-specific guides: OpenClaw browser, agent-browser (CDP connection)                          |

## Prerequisites (for any agent using this skill)

- **API key:** `ALUVIA_API_KEY` from [dashboard.aluvia.io](https://dashboard.aluvia.io). If unset, the agent should stop and ask the user to set it before running Aluvia commands.
- **CLI:** [`npm install -g @aluvia/cli`](https://www.npmjs.com/package/@aluvia/cli) (or `npx aluvia <command>` without a global install).
- **Playwright:** `npm install playwright` (required for browser sessions).

## Installing skills

Use the [skills CLI](https://www.skills.sh/docs/cli) ([`skills` on npm](https://www.npmjs.com/package/skills)). No global install required — `npx` is enough. The CLI detects your agent, symlinks skills into the right directory, and supports [50+ agents](https://www.npmjs.com/package/skills#supported-agents) (Cursor, Claude Code, OpenClaw, Codex, and others).

**Install both skills** (recommended):

```bash
npx skills add aluvia-connect/skills
```

**Install one skill:**

```bash
npx skills add aluvia-connect/skills --skill aluvia
npx skills add aluvia-connect/skills --skill webtomd
```

**Common options:**

```bash
# List skills in this repo without installing
npx skills add aluvia-connect/skills --list

# Install globally (all projects on this machine)
npx skills add aluvia-connect/skills -g

# Target specific agents
npx skills add aluvia-connect/skills -a cursor -a claude-code -a openclaw

# Non-interactive (CI / scripts)
npx skills add aluvia-connect/skills -g -y
```

**From a local clone** (while developing this repo):

```bash
npx skills add ./
npx skills add ./ --skill aluvia
```

After install, use `npx skills list`, `npx skills update`, or `npx skills remove` to manage skills. See the [CLI reference](https://www.skills.sh/docs/cli) for full options.
