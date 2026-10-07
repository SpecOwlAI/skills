# SpecOwl skills for Claude Code

The SpecOwl plugin for Claude Code: 6 skills and the connection to SpecOwl. Version 0.3.1.

## Install

```bash
claude plugin marketplace add SpecOwlAI/skills && claude plugin install specowl@specowl
```

The first time Claude Code uses the connection, a browser opens for you to sign in to SpecOwl and choose the projects and the access the agent gets. If you already connected SpecOwl by hand, remove that connection first: `claude mcp remove contextforge`.

## Skills

- `specowl:find-docs`
- `specowl:implement-ticket`
- `specowl:refine-ticket`
- `specowl:test-ticket`
- `specowl:triage-questions`
- `specowl:write-ticket`

Claude Code loads one on its own when a task matches its description.

## Update

```bash
claude plugin update specowl@specowl
```

Your agent tells you when the installed skills are older than the ones SpecOwl has now.

## Without the marketplace

Download this repo as a .zip, unzip it, and either start Claude Code with `claude --plugin-dir <the unzipped folder>` or copy the folders under `skills/` into `~/.claude/skills/`. A copy gets no updates, and the copied skills alone don't connect: add the connection with `claude mcp add --transport http specowl https://contextforge-na2o.onrender.com/mcp`.

## Your own SpecOwl

The plugin connects to the hosted SpecOwl. To use it with a SpecOwl you host yourself, copy this repo and change the address in `.mcp.json`.

## Changes

This repo is published from SpecOwl's own source and isn't edited by hand: a change made here is overwritten by the next release.
