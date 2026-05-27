# Aluvia Skill

Agent skills that teach AI agents to unblock web requests using [Aluvia](https://aluvia.io) mobile carrier proxies. Format follows the [Agent Skills](https://agentskills.io) standard for use in any compatible tool. When an agent hits 403, Cloudflare, CAPTCHAs, 429s, or IP bans, these skills guide it to route traffic through real US mobile carrier IPs (AT&T, T-Mobile, Verizon) via the Aluvia CLI.

No application code or `package.json` in this repo — it's documentation and `SKILL.md` definitions only.

## What's in this repo

| Directory                    | Purpose                                                                                                               |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `cursor-skill/`              | Cursor IDE skill — when to use Aluvia and how to run the CLI.                                                         |
| `claude-code-skill/`         | Claude Code skill; same content, scoped to `Bash(aluvia *)`.                                                          |
| `openclaw-skill/` | OpenClaw skill with metadata (`bins`, `env`); links to docs and integrations. |
| `docs/`                 | Aluvia CLI reference: command reference, workflows, troubleshooting.                    |
| `docs/integrations/`    | Tool-specific guides: OpenClaw browser, agent-browser (CDP connection).                 |

All three skills cover the same core: installation, prerequisites, commands, standard workflow (start → monitor → rotate IP → close), and safety constraints. OpenClaw’s skill adds a feature list and integration links.

## Prerequisites (for any agent using these skills)

- **API key:** `ALUVIA_API_KEY` from [dashboard.aluvia.io](https://dashboard.aluvia.io). If unset, the agent should stop and ask the user to set it before running Aluvia commands.
- **CLI:** [`npm install -g @aluvia/cli`](https://www.npmjs.com/package/@aluvia/cli) (or `npx aluvia <command>` without a global install).
- **Playwright:** `npm install playwright` (required for browser sessions).

## Using the skills

- **Cursor:** Put the `cursor-skill/` folder (containing `SKILL.md`) in `.cursor/skills/` or your project’s skill path. See [Cursor Agent Skills](https://cursor.com/docs/context/skills).
- **Claude Code:** Put `claude-code-skill/` in `~/.claude/skills/` or `.claude/skills/`. See [Extend Claude with skills](https://code.claude.com/docs/en/skills).
- **OpenClaw:** Put `openclaw-skill/` in `~/.openclaw/skills` or `<workspace>/skills`. See [OpenClaw Skills](https://docs.openclaw.ai/tools/skills).

## Reference docs

- **Aluvia CLI:** [command-reference.md](https://github.com/aluvia-connect/aluvia-skills/blob/main/docs/command-reference.md), [workflows.md](https://github.com/aluvia-connect/aluvia-skills/blob/main/docs/workflows.md), [troubleshooting.md](https://github.com/aluvia-connect/aluvia-skills/blob/main/docs/troubleshooting.md).
- **Integrations:** [OpenClaw browser](https://github.com/aluvia-connect/aluvia-skills/blob/main/docs/integrations/openclaw-browser.md), [agent-browser](https://github.com/aluvia-connect/aluvia-skills/blob/main/docs/integrations/agent-browser.md).

## Quick command reference

| Command                                                              | Purpose                           |
| -------------------------------------------------------------------- | --------------------------------- |
| `aluvia session start <url> --auto-unblock --browser-session <name>` | Start headless browser session.   |
| `aluvia session get --browser-session <name>`                        | Session details and block status. |
| `aluvia session rotate-ip --browser-session <name>`                  | New upstream IP.                  |
| `aluvia session close --browser-session <name>`                      | **Always** close when done.       |
| `aluvia account`                                                     | Account info and balance.         |
| `aluvia geos`                                                        | Available geo regions.            |

Sessions must be explicitly closed; the skills instruct agents to run `session close` (or `session close --all`) on success or failure.
