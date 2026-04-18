# dotfiles

Публично-синхронизируемая часть моего `~/.pi/agent`.

Репозиторий хранит только те Pi-конфиги, которые можно безопасно перенести на другую машину и которые опираются на удалённые пакеты/репозитории. Локальные расширения, сессии, кэши и секреты сюда не попадают.

## Что внутри

- `.pi/agent/AGENTS.md`
- `.pi/agent/settings.json`
- `.pi/agent/models.json`
- `.pi/agent/mcp.json`
- `.pi/agent/steering.json`
- `.pi/agent/pi-permissions.jsonc`
- `.pi/agent/themes/darcula.json`
- `.pi/agent/extensions/pi-tool-display/config.json`
- `INSTALL.md` — пошаговая установка для агента/человека

## Что намеренно не хранится

- `auth.json`
- `codex-accounts.json`
- `sessions/`
- `mcp-cache.json`, `mcp-npx-cache.json`
- `git/` с клонами пакетов
- локальные расширения `pi-cmux-bridge`, `pi-cmux-wait-bridge`
- локальные skills из `~/.pi/agent/skills`
- любые machine-specific или secret файлы

## Быстрая установка

Смотри [INSTALL.md](./INSTALL.md).

Коротко:
1. скопировать файлы из репы в `~/.pi/agent`
2. установить пакеты из `settings.json` через `pi install ...`
3. отдельно заново настроить логины/секреты локально

## Пакеты и расширения

Ниже перечислено, что именно включено и как это устанавливается. Для обычных npm-пакетов команда установки — `pi install <source>`. Для git-пакетов с фильтрами ставится весь пакет, а `settings.json` уже ограничивает, какие extensions реально загружаются.

### Основные пакеты

| Source | Роль | Как установить |
|---|---|---|
| `npm:pi-web-access` | Веб-поиск, fetch URL, GitHub cloning, PDF/YouTube/video analysis. | `pi install npm:pi-web-access` |
| `npm:pi-mcp-adapter` | Подключает MCP-серверы и MCP tools. Нужен для `mcp.json` и `grepai`. | `pi install npm:pi-mcp-adapter` |
| `npm:pi-rewind` | Чекпоинты и откат состояния агента. | `pi install npm:pi-rewind` |
| `npm:@aliou/pi-processes` | Управление фоновыми процессами и наблюдением за ними. | `pi install npm:@aliou/pi-processes` |
| `npm:pi-secret-guard` | Защита от утечки секретов и ключей в git. | `pi install npm:pi-secret-guard` |
| `npm:pi-subagents` | Делегирование задач сабагентам, chain/parallel execution. | `pi install npm:pi-subagents` |
| `npm:pi-boundary` | Спрашивает перед выходом за файловую границу проекта. | `pi install npm:pi-boundary` |
| `npm:@samfp/pi-steering-hooks` | Детерминированные tool guardrails без расхода токенов. | `pi install npm:@samfp/pi-steering-hooks` |
| `npm:pi-mdc-rules` | Загружает и применяет правила из markdown/MDC. | `pi install npm:pi-mdc-rules` |
| `npm:@e9n/pi-github` | GitHub команды/интеграции для PR, issue и review workflow. | `pi install npm:@e9n/pi-github` |
| `npm:pi-tool-display` | Компактный рендер tool calls/results; использует локальный `extensions/pi-tool-display/config.json`. | `pi install npm:pi-tool-display` |
| `git:github.com/garizs/pi-multicodex` | Ротация/управление Codex-аккаунтами. | `pi install git:github.com/garizs/pi-multicodex` |
| `npm:pi-powerline-footer` | Powerline-style footer/status bar. | `pi install npm:pi-powerline-footer` |
| `npm:pi-interview` | Формы/опросники для структурированного сбора вводных. | `pi install npm:pi-interview` |
| `npm:glimpseui` | Нативное micro-UI окно; используется для richer UI/форм. | `pi install npm:glimpseui` |
| `npm:@siddr/pi-cmux-status` | Статус Pi в cmux sidebar/tab workflow. | `pi install npm:@siddr/pi-cmux-status` |
| `npm:pi-permission-system` | Система разрешений, читает `pi-permissions.jsonc`. | `pi install npm:pi-permission-system` |

### Подключённые выборочно расширения из git-пакетов

#### `git:github.com/tmustier/pi-extensions`

Устанавливается целиком:

```bash
pi install git:github.com/tmustier/pi-extensions
```

Но загружаются только такие расширения:

| Extension | Роль |
|---|---|
| `files-widget/index.ts` | Встроенный file browser/review helper; команды вроде `/readfiles`, review/diff workflow. |
| `raw-paste/index.ts` | Команда `/paste` для «сырой» вставки без ломания форматирования. |
| `code-actions/index.ts` | Команда `/code` для выбора, копирования, вставки и запуска snippets из ответов агента. |
| `tab-status/tab-status.ts` | Обновляет title/tab status по состоянию текущего запуска. |

#### `git:github.com/mitsuhiko/agent-stuff`

Устанавливается целиком:

```bash
pi install git:github.com/mitsuhiko/agent-stuff
```

Но загружаются только такие расширения:

| Extension | Роль |
|---|---|
| `extensions/btw.ts` | Side-chat overlay `/btw` для коротких параллельных обсуждений. |
| `extensions/context.ts` | Команда `/context` показывает активный контекст, токены, skills и extensions. |
| `extensions/todos.ts` | File-based todo system в `.pi/todos` с tool и TUI workflow. |

## Внешние зависимости

Не все обязательны, но часть фич без них не раскроется:

- `gh` — для GitHub workflow
- `grepai` — для MCP-сервера из `mcp.json`
- `cmux` — для cmux-интеграций
- `bat`, `git-delta`, `glow` — для `files-widget`
- `tuicr` — для review workflow
- `bun` + `critique` — для diff workflow из `files-widget`

## Принцип репозитория

Этот репозиторий — не полный бэкап `~/.pi`, а переносимая конфигурация:

- remote-backed packages — да
- человекочитаемые настройки — да
- локальные приватные наработки — нет
- секреты и сессии — нет
