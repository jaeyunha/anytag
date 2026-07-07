# AnyTag

AnyTag is an open-source multi-platform chat-agent framework: one agent runtime that can live in Slack, Discord, Telegram, WhatsApp, and future chat surfaces.

The initial product direction is to evolve the OpenTag idea into a reusable framework with:

- platform adapters for multiple chat apps
- pi-mono-style subscription OAuth login alongside API-key auth
- per-user and workspace-level credential resolution
- MCP/add-on integrations
- external/cloud agent backend support
- a future Rust runtime option

Planning starts in [`docs/prds/`](./docs/prds/) before implementation.
