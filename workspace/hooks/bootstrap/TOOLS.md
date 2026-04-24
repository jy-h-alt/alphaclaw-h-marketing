## AlphaClaw Harness

AlphaClaw is the setup and management harness that runs alongside OpenClaw. It provides a web-based Setup UI and manages environment variables, channel connections, Google Workspace integration, and the gateway lifecycle.

AlphaClaw UI: `https://alphaclaw-xlz0.onrender.com`

Do not deflect actionable requests to the Setup UI. If a command or tool is available to you (including OpenClaw CLI commands), execute it yourself first; share Setup UI links only as optional guidance or when the user explicitly asks to do it manually.

### Tabs

| Tab       | URL                          | What it helps with                                                                                                                                                                         |
| --------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| General   | `https://alphaclaw-xlz0.onrender.com#general`   | Gateway status & restart, channel health (Telegram/Discord), pending pairings, feature health (Embeddings/Audio), Google Workspace connection, repo auto-sync schedule, OpenClaw dashboard |
| Watchdog  | `https://alphaclaw-xlz0.onrender.com#watchdog`  | Gateway watchdog lifecycle, crash-loop visibility, restart diagnostics, and auto-repair feature                                                                                            |
| Providers | `https://alphaclaw-xlz0.onrender.com#providers` | AI provider credentials (Anthropic, OpenAI, Gemini, Mistral, Voyage, Groq, Deepgram), feature capabilities, Codex OAuth                                                                    |
| Envars    | `https://alphaclaw-xlz0.onrender.com#envars`    | View/edit/add environment variables (saved to `/data/.env`), gateway restart to apply changes                                                                                              |
| Webhooks  | `https://alphaclaw-xlz0.onrender.com#webhooks`  | Webhook endpoint visibility, create flow, request history, and gateway delivery debugging                                                                                                  |
| Browse    | `https://alphaclaw-xlz0.onrender.com#browse`    | File browser and editor rooted at `.openclaw`, markdown preview/edit flow, and git-aware save workflow                                                                                     |

### Environment variables

Changes to env vars are made through the **Envars** tab (`https://alphaclaw-xlz0.onrender.com#envars`). After saving, a gateway restart may be required to pick up the changes — the UI prompts for this automatically. Do not edit `/data/.env` directly; use the Setup UI so changes are validated and the gateway restart is handled.

### Persistent storage

This deployment runs in an ephemeral container. `/tmp` and other temp locations do not survive redeploys.

Anything persistent must live under `/data/.openclaw`.

For plugins and local tooling:

- Prefer normal `openclaw plugins install <spec>` flows for durable installs.
- If you need to stage a local plugin or helper files first, put them under `/data/.openclaw/...`, not `/tmp/...`.
- Do not leave durable `plugins.load.paths` entries pointing at temp directories.

### Google Workspace

Google Workspace is connected via the **General** tab (`https://alphaclaw-xlz0.onrender.com#general`). The user provides OAuth client credentials from Google Cloud Console, then authorizes access to the services they need (Gmail, Calendar, Drive, Sheets, Docs, Tasks, Contacts, Meet). Connected accounts and `gog` CLI usage are covered by the gog-cli skill.

## Telegram Formatting

- **Links:** Use markdown syntax `[text](URL)` — HTML `<a href>` does NOT render

## Webhooks

You can create webhooks yourself or the user can create them through the AlphaClaw UI.

Webhook transform files must follow this convention:

- Path: hooks/transforms/{hook-name}/{hook-name}-transform.mjs
- Signature: export default async function transform(payload, context)
- Webhook data is at payload.payload (nested)
- Never create transform files outside of hooks/transforms/
- When modifying a transform, read the existing file first

## Topic Registry

When sending messages to group topics, use these thread IDs:

| Group | Topic | Thread ID |
| ----- | ----- | --------- |
| Humanity Marketing (bot) (-1003909100192) | alphaclaw_chat | 6 |

### Sync Rules

When Telegram workspace is enabled, keep topic mappings in sync with real Telegram activity:

- If a message arrives in an unregistered Telegram topic, ask the user to name it for addition to the registry.
- When adding a topic (new or missing) run `alphaclaw telegram topic add --thread <threadId> --name "<topicName>"` immediately, no confirmation needed.
- Never edit `hooks/bootstrap/TOOLS.md` directly for topic changes


## Available Google Accounts

No Google accounts are currently configured.
