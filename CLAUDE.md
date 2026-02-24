# Salesforce Admin Agent

This project contains Claude Code agents for Salesforce admin tasks.

## Commands
- `/admin <task>` — Execute Salesforce admin tasks (deploy, query, manage users, etc.)
- `/orgs <action>` — Manage the org registry (add, remove, list, link, lookup orgs)

## Architecture
- `.claude/commands/admin.md` — The `/admin` dispatcher → reads skill files from `skills/`
- `.claude/commands/orgs.md` — The `/orgs` dispatcher → reads skill files from `skills/orgs/`
- `skills/INDEX.md` — Admin skill index mapping keywords to skill files
- `skills/*.md` — Admin skill files with pre-built SF CLI commands
- `skills/orgs/*.md` — Org registry skill files (add, remove, list, link, lookup)
- `data/org-registry.json` — Persistent org registry with production ↔ sandbox relationships

## Design Principle
Each skill file contains pre-built `sf` CLI commands with placeholder variables. The agent reads only the relevant skill file per request, keeping token usage minimal. The `/admin` agent reads `data/org-registry.json` when it needs to resolve cross-org relationships (e.g., deploying from sandbox to production).

## Connected Orgs
Use `sf org list` to see available orgs. Use `/orgs list` to see the registry with relationships.
