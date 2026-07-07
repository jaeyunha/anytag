# PRD: AnyTag Master Architecture

## 1. Problem

OpenTag proves the value of an AI agent inside Slack, but the broader product should not be Slack-bound or API-key-only. Users should be able to run the same agent across chat platforms, connect their own tools, use either API keys or subscription OAuth where supported, and optionally point the chat shell at another agent backend.

## 2. Goal

Define AnyTag as an open-source, multi-platform chat-agent framework that supports Slack, Discord, Telegram, WhatsApp, future adapters, subscription OAuth login, API-key providers, MCP/add-on integrations, external/cloud agent backends, and a future Rust runtime path.

## 3. Non-goals

- Do not rewrite the full system in Rust as the first milestone.
- Do not require managed cloud hosting for self-hosted users.
- Do not require every platform to have identical UI capabilities.
- Do not ship subscription-token sharing without explicit per-user/workspace policy.

## 4. Users / Actors

- Bot installer / workspace admin
- End user in Slack, Discord, Telegram, or WhatsApp
- Developer adding an integration
- Self-hosted operator
- Managed cloud operator
- External agent provider

## 5. Current Reference State

The OpenTag reference implementation already separates the chat app from the agent runtime:

```txt
chat platform adapter -> shared bot app -> AGENT_URL / AG-UI runtime -> LLM + tools + MCP
```

The target architecture should preserve that separation while making auth, providers, plugins, and external agents first-class.

## 6. Target Behavior

A user can deploy AnyTag on one or more chat platforms and choose a model/auth source per user or workspace:

- API-key provider
- subscription OAuth provider
- workspace default credential
- managed cloud credential
- external agent backend

Developers can add tools through MCP or a plugin interface without editing core platform wiring.

## 7. User Flows

### Multi-platform install

1. Installer configures one or more platform credentials.
2. AnyTag starts one adapter per configured platform.
3. Users mention or DM the bot.
4. The shared agent behavior works across all active platforms.

### Subscription login

1. User sends `/login` in a private chat/DM.
2. User chooses subscription or API key.
3. User chooses provider.
4. AnyTag starts browser OAuth or device-code login.
5. Credentials are stored under the user/workspace auth scope.
6. Token refresh happens automatically.

### External agent backend

1. Operator sets an external `AGENT_URL` or equivalent backend route.
2. AnyTag forwards chat turns to that backend.
3. The backend returns AG-UI-compatible events or a supported adapter response.

## 8. Technical Requirements

- Platform adapters must remain replaceable.
- Runtime must not hardcode one model provider.
- Auth resolution must support OAuth, API keys, env fallback, workspace fallback, and per-user credentials.
- Token refresh must be serialized to avoid concurrent refresh races.
- Sensitive values must be redacted in logs and errors.
- Plugin/tool registration must be declarative.
- Rust runtime must be planned as an optional backend, not a mandatory rewrite.

## 9. Security / Privacy Requirements

- OAuth login must happen in private surfaces only.
- Per-user credentials must not leak across users.
- Workspace-default credentials require explicit admin policy.
- Tokens must be encrypted at rest for hosted/multi-user deployments.
- Logs must never include access tokens, refresh tokens, API keys, or authorization headers.
- Each tool/write action must preserve human-in-the-loop approval semantics where applicable.

## 10. Platform Differences

- Slack has threads, app mentions, modals, and assistant surfaces.
- Discord has guilds, privileged intents, slash commands, and component limits.
- Telegram has long-polling, simpler UI, and no native modal equivalent.
- WhatsApp requires webhook verification and has stricter message/template constraints.

Each platform should expose the best native UX without blocking the shared core.

## 11. Acceptance Criteria

- The PRD pack is split into independently plannable PRDs.
- Each sub-PRD has testable acceptance criteria and verification steps.
- Auth, platform, runtime, plugin, external-agent, security, Rust, and release concerns are separated.
- The execution sequence avoids Rust/full-cloud work before provider/auth/platform foundations are planned.

## 12. Verification Plan

Ralplan must review this PRD for:

- architecture sequencing correctness
- missing security boundaries
- provider/auth abstraction risks
- platform parity assumptions
- Rust scope containment
- concrete acceptance criteria for each sub-PRD

## 13. Open Questions

- Should the first implementation target per-user auth only, or also workspace defaults?
- Which OAuth providers are allowed in the initial open-source release?
- Should credential storage be local-file first, database first, or pluggable from day one?
- What minimum AG-UI compatibility is required for external agent backends?

## 14. Rollout Plan

1. Run this master PRD through `/skill:ralplan --deliberate`.
2. Finalize the sub-PRD boundaries.
3. Run auth/security PRDs before implementation.
4. Run multi-platform and runtime abstraction PRDs next.
5. Plan plugin and external-agent interop.
6. Plan Rust runtime only after runtime/provider boundaries are stable.

## 15. Risks

- Provider OAuth terms may change.
- Subscription auth can create account/quota ambiguity.
- Multi-platform UI parity can overconstrain the core.
- Rust rewrite pressure can delay the simpler working architecture.
- Poor credential scoping can create serious security bugs.
