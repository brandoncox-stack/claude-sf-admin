# Salesforce Admin Agent

This project contains a Claude Code agent for Salesforce admin tasks.

## Usage
Run `/admin <task description>` in Claude Code to execute Salesforce admin tasks.

## Architecture
- `.claude/commands/admin.md` — The `/admin` slash command (dispatcher)
- `skills/INDEX.md` — Skill index mapping keywords to skill files
- `skills/*.md` — Individual skill files with exact SF CLI commands

## Design Principle
Each skill file contains pre-built `sf` CLI commands with placeholder variables. The agent reads only the relevant skill file per request, keeping token usage minimal.

## Connected Orgs
Use `sf org list` to see available orgs. Always specify `--target-org` in commands.
