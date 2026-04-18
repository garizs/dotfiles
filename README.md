# dotfiles

Portable Pi dotfiles extracted from my `~/.pi/agent`.

This repository contains only the Pi configuration I can safely move across machines: human-readable settings, themes, and remote-backed package declarations. It intentionally does **not** include secrets, auth state, caches, session history, or local-only unpublished extensions.

Русская версия: [README.ru.md](./README.ru.md)

## Contents

- `.pi/agent/AGENTS.md`
- `.pi/agent/settings.json`
- `.pi/agent/models.json`
- `.pi/agent/mcp.json`
- `.pi/agent/steering.json`
- `.pi/agent/pi-permissions.jsonc`
- `.pi/agent/themes/darcula.json`
- `.pi/agent/extensions/pi-tool-display/config.json`
- `INSTALL.md` — agent-oriented installation guide

## Intentionally excluded

- `auth.json`
- `codex-accounts.json`
- `sessions/`
- `mcp-cache.json`, `mcp-npx-cache.json`
- `git/` package clones
- local extensions `pi-cmux-bridge`, `pi-cmux-wait-bridge`
- local skills from `~/.pi/agent/skills`
- any machine-specific or secret files

## Quick install

See [INSTALL.md](./INSTALL.md).

In short:
1. copy tracked files from this repo into `~/.pi/agent`
2. install the packages declared in `settings.json`
3. recreate logins and secrets locally on the target machine

## Packages and extensions

For regular npm packages, install with `pi install <source>`. For filtered git packages, install the full package; `settings.json` already limits which extensions are actually loaded.

### Core packages

| Source | Role | Install |
|---|---|---|
| `npm:pi-web-access` | Web search, URL fetch, GitHub cloning, PDF/YouTube/video analysis. | `pi install npm:pi-web-access` |
| `npm:pi-mcp-adapter` | Connects MCP servers and exposes MCP tools. Required for `mcp.json` and `grepai`. | `pi install npm:pi-mcp-adapter` |
| `npm:pi-rewind` | Checkpoints and rewind support for agent state. | `pi install npm:pi-rewind` |
| `npm:@aliou/pi-processes` | Background process management and monitoring. | `pi install npm:@aliou/pi-processes` |
| `npm:pi-secret-guard` | Prevents accidentally committing secrets and credentials. | `pi install npm:pi-secret-guard` |
| `npm:pi-subagents` | Subagent delegation, chains, and parallel execution. | `pi install npm:pi-subagents` |
| `npm:pi-boundary` | Prompts before the agent leaves the current project boundary. | `pi install npm:pi-boundary` |
| `npm:@samfp/pi-steering-hooks` | Deterministic tool-call guardrails with zero token cost. | `pi install npm:@samfp/pi-steering-hooks` |
| `npm:pi-mdc-rules` | Loads and enforces markdown/MDC-based rules. | `pi install npm:pi-mdc-rules` |
| `npm:@e9n/pi-github` | GitHub integration for PR, issue, and review workflows. | `pi install npm:@e9n/pi-github` |
| `npm:pi-tool-display` | Compact tool-call/result rendering; uses the local `extensions/pi-tool-display/config.json`. | `pi install npm:pi-tool-display` |
| `git:github.com/garizs/pi-multicodex` | Codex account rotation/management. | `pi install git:github.com/garizs/pi-multicodex` |
| `npm:pi-powerline-footer` | Powerline-style footer/status bar. | `pi install npm:pi-powerline-footer` |
| `npm:pi-interview` | Structured forms/questionnaires for collecting input. | `pi install npm:pi-interview` |
| `npm:glimpseui` | Native micro-UI window used by richer UI flows. | `pi install npm:glimpseui` |
| `npm:@siddr/pi-cmux-status` | Pi status integration for cmux sidebar/tab workflows. | `pi install npm:@siddr/pi-cmux-status` |
| `npm:pi-permission-system` | Permission system reading `pi-permissions.jsonc`. | `pi install npm:pi-permission-system` |

### Selectively loaded extensions from git packages

#### `git:github.com/tmustier/pi-extensions`

Install the package:

```bash
pi install git:github.com/tmustier/pi-extensions
```

Only these extensions are enabled:

| Extension | Role |
|---|---|
| `files-widget/index.ts` | In-terminal file browser/review helper with commands like `/readfiles` plus review/diff workflow helpers. |
| `raw-paste/index.ts` | `/paste` command for raw paste without breaking formatting. |
| `code-actions/index.ts` | `/code` command to pick, copy, insert, and run snippets from assistant replies. |
| `tab-status/tab-status.ts` | Updates the terminal/tab title with current run status. |

#### `git:github.com/mitsuhiko/agent-stuff`

Install the package:

```bash
pi install git:github.com/mitsuhiko/agent-stuff
```

Only these extensions are enabled:

| Extension | Role |
|---|---|
| `extensions/btw.ts` | `/btw` side-chat overlay for short parallel discussions. |
| `extensions/context.ts` | `/context` command showing active context, tokens, skills, and extensions. |
| `extensions/todos.ts` | File-based todo system in `.pi/todos` with tool and TUI workflow. |

## External dependencies

Not all of these are mandatory, but some configured features depend on them:

- `gh` — GitHub workflow integration
- `grepai` — MCP server configured in `mcp.json`
- `cmux` — cmux integrations
- `bat`, `git-delta`, `glow` — used by `files-widget`
- `tuicr` — review workflow
- `bun` + `critique` — diff workflow from `files-widget`

## Repository policy

This is not a full backup of `~/.pi`.
It is a **portable Pi configuration** repository:

- remote-backed packages: yes
- readable settings and themes: yes
- local private experiments: no
- secrets and session history: no
