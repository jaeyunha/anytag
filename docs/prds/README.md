# AnyTag PRD Pack

This directory holds product requirements documents that should each go through `/skill:ralplan --deliberate` before implementation.

## PRD sequence

1. `000-master-architecture.md` — product north star and staged PRD pack.
2. `001-multi-platform-chat.md` — Slack, Discord, Telegram, WhatsApp, and future adapters.
3. `002-subscription-oauth-auth-broker.md` — pi-mono-style subscription login plus API-key auth.
4. `003-provider-runtime-abstraction.md` — provider/model/auth resolver architecture.
5. `004-external-agent-interop.md` — AGENT_URL, AG-UI, MCP, HTTP, and managed cloud agents.
6. `005-addon-plugin-system.md` — MCP/tool/plugin extension model.
7. `006-multitenant-security-storage.md` — credential ownership, encryption, audit, and policy.
8. `007-rust-runtime-option.md` — optional Rust backend/runtime strategy.
9. `008-tests-release-docs.md` — verification, e2e, docs, and rollout.

## Ralplan command

```txt
/skill:ralplan --deliberate "<PRD text or PRD file path>"
```

For underspecified PRDs, run `/skill:deep-interview --standard` first, then feed the resulting spec into ralplan.
