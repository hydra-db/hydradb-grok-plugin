# HydraDB Grok Build Plugin

Automatic long-term memory for [Grok Build](https://docs.x.ai/build/overview)
(xAI's coding agent), powered by [HydraDB](https://hydradb.com).

Grok Build's plugin system is Claude-Code-compatible (`.grok-plugin/plugin.json`
+ `hooks/hooks.json` with `${CLAUDE_PLUGIN_ROOT}` and `additionalContext`
injection), so this is a full plugin on the same shared engine
(`scripts/plugin.mjs`) as the Claude Code / Codex plugins - with real per-prompt
auto-recall, not just MCP tools.

## What it does

| Grok Build hook | Engine action |
|---|---|
| `SessionStart` | print status + async workspace sync |
| `UserPromptSubmit` | recall relevant memory/knowledge, inject as `additionalContext` |
| `PostToolUse` (file writes) | incremental workspace sync |
| `Stop` | capture the completed turn |

## Prerequisites

- Node.js >= 18
- A HydraDB account: API key + tenant ID ([hydradb.com](https://hydradb.com))
- Grok Build installed

## Install

```bash
export HYDRADB_API_KEY="your-api-key"
export HYDRADB_TENANT_ID="your-tenant-id"
```

Install the plugin in Grok Build (from a local checkout, or once listed, via the
marketplace):

```
/marketplace        # browse and install
```

To publish to the official catalog, submit a PR adding this repo to
[`xai-org/plugin-marketplace`](https://github.com/xai-org/plugin-marketplace).

## Configuration

Config keys, environment overrides, and capture/search/ingest modes are identical
to the other HydraDB plugins - see `config.example.json`.

## MCP alternative

If you only want pull-style recall (agent-invoked tools), skip the plugin and add
the MCP server instead: `grok mcp add hydradb --url https://mcp.hydradb.com`, or a
custom connector at `grok.com/connectors` (see `grok-mcp.snippet.json`). The plugin
already gives automatic recall, so MCP is optional here.

## License

Apache-2.0 - Copyright (c) 2026 HydraDB
