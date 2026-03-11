# Gologin Local Agent Browser Skill

## Install This Skill

Install the skill with:

```bash
npx skills add GologinLabs/gologin-local-agent-browser-skill
```

Standalone repo:

- [GologinLabs/gologin-local-agent-browser-skill](https://github.com/GologinLabs/gologin-local-agent-browser-skill)

## Required CLI

This skill is built around the `gologin-local-agent-browser-cli` package and the `gologin-local-agent-browser` command.

Install the CLI globally:

```bash
npm install -g gologin-local-agent-browser-cli
```

CLI repo:

- [GologinLabs/gologin-local-agent-browser](https://github.com/GologinLabs/gologin-local-agent-browser)

## Overview

Gologin Local Agent Browser Skill is for persistent local GoLogin Orbita automation with daemon-backed sessions, snapshots, refs, runbooks, and batch execution.

It is built for:

- opening local GoLogin profile sessions
- keeping cookies and browser state inside persistent profiles
- profile warmup and login flows
- ref-based page interaction after `snapshot`
- screenshots, PDFs, uploads, and session inspection
- reusable `run` and `batch` automations with local job history

## Capabilities

- Session lifecycle with `open`, `close`, `sessions`, and `current`
- Runbooks and batches with `run`, `batch`, `jobs`, and `job`
- Snapshot-driven interaction with `snapshot`, `click`, `type`, and `fill`
- Semantic actions with `find`
- State helpers with `cookies`, `storage-export`, `storage-import`, and `eval`
- Persistent profile management with `profiles`, `profile-create`, `profile-import`, and `profile-update`

## Setup

Set your token:

```bash
export GOLOGIN_API_TOKEN="your_gologin_token"
export GOLOGIN_PROFILE_ID="optional_profile_id"
```

Optional environment variables:

- `GOLOGIN_HEADLESS`
- `GOLOGIN_DAEMON_PORT`
- `GOLOGIN_EXECUTABLE_PATH`
- `GOLOGIN_TMPDIR`

## Quickstart

```bash
gologin-local-agent-browser open https://example.com --profile your_profile_id --headless
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser click @e3
gologin-local-agent-browser close
```

## When To Use This Skill

Use this skill when:

- the task must run inside a local GoLogin profile
- cookies, storage, and browsing state should persist across runs
- the agent needs to click, type, navigate, or save artifacts
- profile warmup, login routines, or runbook-based automation are required

## References

- [`SKILL.md`](./SKILL.md)
- [`references/command-map.md`](./references/command-map.md)
- [`workflows/`](./workflows)
