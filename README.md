# SpecOwl skills for Claude Code

The SpecOwl plugin for Claude Code: 8 skills and the connection to SpecOwl. Version 0.10.0.

## Install

```bash
claude plugin marketplace add SpecOwlAI/skills && claude plugin install specowl@specowl
```

The first time Claude Code uses the connection, a browser opens for you to sign in to SpecOwl and choose the projects and the access the agent gets. If you already connected SpecOwl by hand, remove that connection first: `claude mcp remove contextforge`.

## Skills

- `specowl:find-docs`
- `specowl:implement-ticket`
- `specowl:plan-epic`
- `specowl:refine-ticket`
- `specowl:review-roadmap`
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

Download this repo as a .zip, unzip it, and either start Claude Code with `claude --plugin-dir <the unzipped folder>` or copy the folders under `skills/` into `~/.claude/skills/`. A copy gets no updates, and the copied skills alone don't connect: add the connection with `claude mcp add --transport http specowl https://contextforge-na2o.onrender.com/mcp`, or your own SpecOwl's address followed by `/mcp`.

## Your own SpecOwl

The plugin connects to the hosted SpecOwl unless `SPECOWL_URL` names another. To use it with a SpecOwl your company hosts, add its address to the `env` of Claude Code's settings (`~/.claude/settings.json`), as its Agents page shows, then start Claude Code again:

```json
{ "env": { "SPECOWL_URL": "https://specowl.example.com" } }
```

Write the address the way the Agents page shows it: no `/mcp` and no slash at the end. Remove the line, rather than leave it empty, to go back to the hosted SpecOwl.

## Changes

This repo is published from SpecOwl's own source and isn't edited by hand: a change made here is overwritten by the next release.
