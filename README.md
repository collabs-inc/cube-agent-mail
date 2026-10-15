# Agent-Native Mail for Cube

Connect your mailbox and manage messages and email rules. This is a standalone adaptation of [Agent-Native Mail](https://www.agent-native.com/apps/mail/), built by [Builder.io](https://github.com/BuilderIO/agent-native). Upstream source provenance is recorded in [UPSTREAM.json](UPSTREAM.json); the original guide is [UPSTREAM-README.md](UPSTREAM-README.md). Cube integration recipes are maintained in [cube-agent-native-apps](https://github.com/collabs-inc/cube-agent-native-apps).

## Install in Cube

Use Cube’s Add app from repo with https://github.com/collabs-inc/cube-agent-mail. Cube installs dependencies, builds the app, and runs it behind its authenticated app gate. Node.js 22 or newer is required.

For a manual local check, run these commands from the checkout:

```sh
node cube/install.mjs
PORT=3000 node cube/start.mjs
```

Open http://127.0.0.1:3000. The server binds only to loopback. The local owner session relies on Cube’s gate for authentication; do not expose the raw server directly to the internet.

## Configure AI and connections

Open **Connect AI** or the app’s model settings and configure a supported provider. API keys are not included. Cube’s Claude/Codex CLI login is not automatically shared with this app’s provider runtime. Without a provider, manual editing and local actions remain available, but AI generation is unavailable.

Provider environment configuration can be kept in `~/.local/share/cube-agent-mail/config.env` (or under `$XDG_DATA_HOME/cube-agent-mail`). Restrict this file to your user, for example with `chmod 600`. Do not commit credentials.

Mail sync and sending require a connected mailbox. Integration tests do not send email.

## Data and updates

Database, blobs, uploads, and runtime state live under `$XDG_DATA_HOME/cube-agent-mail`, defaulting to `~/.local/share/cube-agent-mail`. The app checkout contains links to this state. Cube updates can replace the checkout without deleting it. Stop the app before backing up the complete data directory; keep its `auth-secret` alongside the database so encrypted references stay readable. Uninstalling leaves this directory intact.

## Development

Run `npm exec --yes --package=pnpm@10.29.1 -- pnpm install --frozen-lockfile` and `node cube/build.mjs`. The runtime uses a production Nitro build and a per-app PGlite database through Agent-Native’s embedded-host API. Local storage replaces hosted blob requirements. See the integration repository for verification results and upstream compatibility patches.

## Attribution

Upstream declares the MIT license. Original application code belongs to Builder.io and its contributors; Cube packaging and compatibility changes are maintained by collabs-inc.
