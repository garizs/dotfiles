# INSTALL.md

Agent-oriented installation guide for these Pi dotfiles.

## Goal

Recreate the tracked part of this repo into `~/.pi/agent` on a new machine **without** copying secrets, auth state, session history, or local-only extensions.

## What to install

Copy these files from the repository into `~/.pi/agent`:

- `.pi/agent/AGENTS.md`
- `.pi/agent/settings.json`
- `.pi/agent/models.json`
- `.pi/agent/mcp.json`
- `.pi/agent/steering.json`
- `.pi/agent/pi-permissions.jsonc`
- `.pi/agent/themes/darcula.json`
- `.pi/agent/extensions/pi-tool-display/config.json`

Do **not** copy these from another machine:

- `auth.json`
- `codex-accounts.json`
- `sessions/`
- `mcp-cache.json`
- `mcp-npx-cache.json`
- `git/`
- local extensions `pi-cmux-bridge` and `pi-cmux-wait-bridge`
- local skills from `~/.pi/agent/skills`

## Prerequisites

Install these first:

- Node.js + `pi`
- `gh` (GitHub CLI)
- optional but recommended: `grepai`, `cmux`, `bat`, `git-delta`, `glow`, `bun`, `tuicr`

## Step 1: back up the current Pi config

If `~/.pi/agent` already exists, move it out of the way first:

```bash
mkdir -p ~/.pi
mv ~/.pi/agent ~/.pi/agent.backup
```

Then recreate the target directory:

```bash
mkdir -p ~/.pi/agent
```

If you do not want to replace the whole directory, back up only the files that will be overwritten.

## Step 2: copy tracked files from this repo

Run these commands from the root of this repository:

```bash
mkdir -p ~/.pi/agent/themes
mkdir -p ~/.pi/agent/extensions/pi-tool-display

cp .pi/agent/AGENTS.md ~/.pi/agent/AGENTS.md
cp .pi/agent/settings.json ~/.pi/agent/settings.json
cp .pi/agent/models.json ~/.pi/agent/models.json
cp .pi/agent/mcp.json ~/.pi/agent/mcp.json
cp .pi/agent/steering.json ~/.pi/agent/steering.json
cp .pi/agent/pi-permissions.jsonc ~/.pi/agent/pi-permissions.jsonc
cp .pi/agent/themes/darcula.json ~/.pi/agent/themes/darcula.json
cp .pi/agent/extensions/pi-tool-display/config.json ~/.pi/agent/extensions/pi-tool-display/config.json
```

## Step 3: install remote-backed Pi packages

Install every package referenced by `settings.json` except local-only ones.

```bash
pi install npm:pi-web-access
pi install npm:pi-mcp-adapter
pi install npm:pi-rewind
pi install npm:@aliou/pi-processes
pi install npm:pi-secret-guard
pi install npm:pi-subagents
pi install npm:pi-boundary
pi install npm:@samfp/pi-steering-hooks
pi install npm:pi-mdc-rules
pi install npm:@e9n/pi-github
pi install npm:pi-tool-display
pi install git:github.com/garizs/pi-multicodex
pi install npm:pi-powerline-footer
pi install git:github.com/tmustier/pi-extensions
pi install git:github.com/mitsuhiko/agent-stuff
pi install npm:pi-interview
pi install npm:glimpseui
pi install npm:@siddr/pi-cmux-status
pi install npm:pi-permission-system
```

Notes:

- `settings.json` already filters `tmustier/pi-extensions` and `mitsuhiko/agent-stuff` down to the specific extensions used here.
- Do not re-add the old local path package for `pi-cmux-bridge`; it is intentionally excluded.

## Step 4: reinstall external non-Pi tools if needed

These are optional, but some configured extensions depend on them:

```bash
brew install gh
brew install bat git-delta glow
brew install oven-sh/bun/bun
brew install grepai
```

Additional tools you may want manually:

- `cmux`
- `tuicr`
- `critique` (usually via `bunx critique` workflow)

## Step 5: restore machine-local auth and secrets manually

Do this locally on the target machine. Never sync these files from another machine.

- Run `pi` and use `/login`
- Recreate any Codex/OpenAI/GitHub auth that your setup needs
- Recreate `auth.json` and `codex-accounts.json` only through normal login/setup flows

## Step 6: verify

After installation:

1. Run `pi list` and confirm all packages are present
2. Start `pi`
3. Confirm the active theme is `darcula`
4. Check that these commands/tools exist:
   - `/context`
   - `/btw`
   - `/code`
   - `/readfiles`
   - interview tool
   - todo tool
5. If `grepai` is installed, confirm MCP tools are available through the `grepai` server

## Expected behavior after install

- Pi uses `openai-codex / gpt-5.4` by default
- thinking level defaults to `high`
- theme is `darcula`
- steering rules block shell patterns like redirection, variable expansion, inline scripts, and network fetches
- permission policy defaults to permissive file tools with `bash` on ask
- `pi-tool-display` uses the local config stored in this repo

## If something is missing

- missing commands from `pi-extensions` or `agent-stuff`: verify that `settings.json` was copied exactly
- missing MCP tools: verify `grepai` is installed and on `PATH`
- missing GitHub features: verify `gh auth status`
- missing interview/native windows: verify `glimpseui` installed correctly
- permission prompts behaving strangely: verify `~/.pi/agent/pi-permissions.jsonc`

## Scope policy for this repo

This repo is intentionally limited to:

- portable config files
- remote-backed package declarations
- theme/config overrides

It intentionally excludes:

- secrets
- sessions/history
- caches
- local-only unpublished extensions and skills
