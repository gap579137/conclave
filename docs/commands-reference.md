# Conclave Commands Reference

This reference is derived from the current CLI surface (`conclave --help`).

Last verified: **February 18, 2026**.

## Top-Level Commands

| Command | Purpose |
|---|---|
| `onboard` | Initialize workspace/config quickly or interactively |
| `agent` | Run interactive chat or single-message mode |
| `gateway` | Start webhook and WhatsApp HTTP gateway |
| `daemon` | Start supervised runtime (gateway + channels + optional heartbeat/scheduler) |
| `service` | Manage user-level OS service lifecycle |
| `doctor` | Run diagnostics and freshness checks |
| `status` | Print current configuration and system summary |
| `cron` | Manage scheduled tasks |
| `models` | Refresh provider model catalogs |
| `providers` | List provider IDs, aliases, and active provider |
| `channel` | Manage channels and channel health checks |
| `integrations` | Inspect integration details |
| `skills` | List/install/remove skills |
| `migrate` | Import from external runtimes (currently OpenClaw) |
| `hardware` | Discover and introspect USB hardware |
| `peripheral` | Configure and flash peripherals |
| `auth` | Manage OAuth and token-based authentication profiles |

## Command Groups

### `onboard`

- `conclave onboard`
- `conclave onboard --interactive`
- `conclave onboard --channels-only`
- `conclave onboard --api-key <KEY> --provider <ID> --memory <sqlite|lucid|markdown|none>`

### `agent`

- `conclave agent`
- `conclave agent -m "Hello"`
- `conclave agent --provider <ID> --model <MODEL> --temperature <0.0-2.0>`
- `conclave agent --peripheral <board:path>`

### `gateway` / `daemon`

- `conclave gateway [--host <HOST>] [--port <PORT>]`
- `conclave daemon [--host <HOST>] [--port <PORT>]`

### `service`

- `conclave service install`
- `conclave service start`
- `conclave service stop`
- `conclave service status`
- `conclave service uninstall`

### `cron`

- `conclave cron list`
- `conclave cron add <expr> [--tz <IANA_TZ>] <command>`
- `conclave cron add-at <rfc3339_timestamp> <command>`
- `conclave cron add-every <every_ms> <command>`
- `conclave cron once <delay> <command>`
- `conclave cron remove <id>`
- `conclave cron pause <id>`
- `conclave cron resume <id>`

### `models`

- `conclave models refresh`
- `conclave models refresh --provider <ID>`
- `conclave models refresh --force`

### `channel`

- `conclave channel list`
- `conclave channel start`
- `conclave channel doctor`
- `conclave channel bind-telegram <IDENTITY>`
- `conclave channel add <type> <json>`
- `conclave channel remove <name>`

Runtime in-chat commands (Telegram/Discord while channel server is running):

- `/models`
- `/models <provider>`
- `/model`
- `/model <model-id>`

`add/remove` currently route you back to managed setup/manual config paths (not full declarative mutators yet).

### `integrations`

- `conclave integrations info <name>`

### `skills`

- `conclave skills list`
- `conclave skills install <source>`
- `conclave skills remove <name>`

### `migrate`

- `conclave migrate openclaw [--source <path>] [--dry-run]`

### `hardware`

- `conclave hardware discover`
- `conclave hardware introspect <path>`
- `conclave hardware info [--chip <chip_name>]`

### `peripheral`

- `conclave peripheral list`
- `conclave peripheral add <board> <path>`
- `conclave peripheral flash [--port <serial_port>]`
- `conclave peripheral setup-uno-q [--host <ip_or_host>]`
- `conclave peripheral flash-nucleo`

### `auth`

- `conclave auth login --provider openai-codex` — Browser OAuth flow (prompts for environment type)
- `conclave auth login --provider openai-codex --headless` — Headless mode: paste callback URL manually
- `conclave auth login --provider openai-codex --device-code` — Device-code flow (recommended for servers)
- `conclave auth paste-redirect --provider openai-codex --profile <name>` — Complete OAuth by pasting callback URL
- `conclave auth paste-token --provider anthropic --profile <name>` — Store API key or auth token
- `conclave auth setup-token --provider anthropic` — Alias for paste-token with authorization mode
- `conclave auth refresh --provider openai-codex --profile <name>` — Refresh access token
- `conclave auth logout --provider <provider> --profile <name>` — Remove auth profile
- `conclave auth use --provider <provider> --profile <name>` — Set active profile
- `conclave auth list` — List all auth profiles
- `conclave auth status` — Show auth status with token expiry info

## Validation Tip

To verify docs against your current binary quickly:

```bash
conclave --help
conclave <command> --help
```
