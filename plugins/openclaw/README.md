# OpenClaw Plugin

Skill for configuring and managing [OpenClaw](https://docs.openclaw.ai) — the self-hosted gateway that connects chat platforms (Discord, Telegram, WhatsApp, Slack, Signal, iMessage, Matrix, Microsoft Teams, and more) to AI coding agents.

## What it does

- Guides installation across platforms (macOS, Docker, Kubernetes, Node.js, etc.)
- Walks through channel setup with secure token management (SecretRef)
- Configures multi-agent routing with bindings and tool policies
- Troubleshoots gateway issues using OpenClaw-specific diagnostics
- References live documentation to stay current as OpenClaw evolves

## When it triggers

Mentions of OpenClaw, `openclaw` CLI commands, gateway configuration, chat channel setup for AI agents, SOUL.md/IDENTITY.md files, or multi-agent routing.

## Design philosophy

The skill is intentionally lightweight — it teaches the agent *how* to work with OpenClaw and *where* to find details, rather than bundling the full documentation. This keeps the skill current as OpenClaw evolves rapidly.
