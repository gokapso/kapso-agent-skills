# Kapso Agent Skills

> **Alpha**: These skills are in active development and subject to rapid change.

Agent skills for [Kapso](https://kapso.ai), built on the open [Agent Skills](https://agentskills.io) format.

## Installation

```bash
npx skills add gokapso/kapso-agent-skills
```

## What are Agent Skills?

Agent Skills are folders of instructions, scripts, and resources that agents can discover and use to perform tasks more accurately. Each skill is a directory with a `SKILL.md` entrypoint and optional supporting files.

```
my-skill/
├── SKILL.md          # Required: instructions + metadata
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
└── assets/           # Optional: templates, resources
```

Skills use **progressive disclosure**: agents load only the name and description at startup, then read full instructions when a task matches. This keeps context usage efficient while giving agents access to detailed knowledge on demand.

## Available skills

- **kapso-automation**: Manage workflows, functions, databases, and integrations
- **whatsapp-messaging**: Send messages, create templates, handle interactive messages
- **whatsapp-flows**: Build and deploy WhatsApp Flows with data endpoints
- **kapso-api**: Customer management and WhatsApp connection setup
- **kapso-ops**: Debug and troubleshoot Kapso projects

Each skill contains detailed documentation in its `SKILL.md` file.

## SKILL.md format

Each skill requires a `SKILL.md` file with YAML frontmatter:

```yaml
---
name: my-skill
description: What this skill does and when to use it.
---

# My Skill

Instructions for the agent...
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Lowercase identifier (letters, numbers, hyphens) |
| `description` | Yes | When to use this skill (max 1024 chars) |

## Learn more

- [Agent Skills specification](https://agentskills.io/specification)
- [Authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Example skills](https://github.com/anthropics/skills)
