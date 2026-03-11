---
name: gologin-local-agent-browser-skill
description: Use this skill when an agent should automate websites through a local GoLogin Orbita profile instead of a regular local browser or Gologin Cloud Browser. Covers profile warmup, login flows, cookie collection, persistent account sessions, screenshots, PDFs, and ref-based interaction through the gologin-local-agent-browser CLI. Trigger when the user mentions local GoLogin, Orbita, profile warmup, account login automation, cookie harvesting, Reddit/account routines, or reuse of an existing GoLogin profile on this machine.
---

# Gologin Local Agent Browser Skill

## Overview

Use the `gologin-local-agent-browser` CLI as the single interface for local GoLogin browser automation. Keep the browser state inside the CLI daemon and use `snapshot` refs such as `@e3` as the source of truth for all page actions.

## Core Rules

- `GOLOGIN_TOKEN` or `GOLOGIN_API_TOKEN` is mandatory for any runtime action through this skill.
- Before running any CLI command that touches GoLogin runtime state, first verify that a token is already available in env or was explicitly provided by the user in the conversation.
- If the token is missing, stop immediately and ask the user for it. Do not try to "work around" the missing token.
- Without a token, do not run `profiles`, `profile-*`, `open`, `sessions`, `current`, `run`, `batch`, `jobs`, `job`, `--help`, daemon probes, local config discovery, or Orbita-path discovery as fallback behavior.
- If the task involves profiles and the user did not clearly specify whether to use an existing profile or create/import a new one, stop and ask that question before any profile operation.
- Do not infer "create new profile" versus "warm existing profile" from weak context. Ask explicitly unless the user already made that choice.
- Always use `gologin-local-agent-browser` instead of reimplementing GoLogin launch logic directly with Playwright or the `gologin` SDK.
- Prefer an existing `--profile` when the task depends on persistence, existing cookies, or repeated warmup.
- Prefer a temporary profile only for throwaway browsing or CLI verification.
- Use `--headless` for automation by default. Use `--headed` only when visual debugging is necessary.
- Treat the latest `snapshot` as authoritative. After navigation or DOM-changing actions, run `snapshot` again before reusing refs.
- Close the session with `close` when the task is done so GoLogin commits profile state back to storage.
- Use this skill only for profiles, accounts, and websites the user is authorized to control.

## CLI Resolution

Use the globally installed CLI:

```bash
gologin-local-agent-browser <command> ...
```

If it is not installed yet:

```bash
npm install -g gologin-local-agent-browser-cli
```

If you are working from a cloned source checkout instead:

```bash
git clone https://github.com/GologinLabs/gologin-local-agent-browser.git
cd gologin-local-agent-browser
npm install
npm run build
node ./dist/cli.js <command> ...
```

## Setup

Expect these environment variables:

- `GOLOGIN_TOKEN` or `GOLOGIN_API_TOKEN`
- `GOLOGIN_PROFILE_ID` for a default persistent profile
- `GOLOGIN_HEADLESS` for default headless mode
- `GOLOGIN_EXECUTABLE_PATH` only if Orbita is not in the expected SDK location
- `GOLOGIN_TMPDIR` only if profile temp data should live in a custom directory

Blocking preflight:

- If no GoLogin token is available, ask the user for `GOLOGIN_TOKEN` or `GOLOGIN_API_TOKEN` before any runtime action.
- `GOLOGIN_PROFILE_ID` is optional. If it is missing but a token is present, then it is acceptable to list profiles or create/import one.
- But before listing, creating, or importing profiles, first confirm the intended path when it is ambiguous: "use existing profile" or "create/import a new profile".

## Command Map

Use these commands directly:

- `open <url> [--profile <profileId>] [--session <sessionId>] [--idle-timeout-ms <ms>] [--headless|--background|--headed|--visible]`
- `run <runbook.json> [--session <sessionId>] [--profile <profileId>] [--vars <variables.json>] [--name <jobName>] [--continue-on-error] [--json]`
- `batch <runbook.json> --targets <targets.json> [--concurrency <n>] [--vars <variables.json>] [--name <jobName>] [--continue-on-error] [--json]`
- `jobs [--kind <run|batch>] [--status <running|ok|partial|failed>] [--search <text>] [--limit <n>] [--json]`
- `job <jobId> [--json]`
- `snapshot [--session <sessionId>] [--interactive|-i]`
- `click <target> [--session <sessionId>]`
- `dblclick <target> [--session <sessionId>]`
- `focus <target> [--session <sessionId>]`
- `type <target> <text> [--session <sessionId>]`
- `fill <target> <text> [--session <sessionId>]`
- `hover <target> [--session <sessionId>]`
- `select <target> <value> [--session <sessionId>]`
- `check <target> [--session <sessionId>]`
- `uncheck <target> [--session <sessionId>]`
- `press <key> [target] [--session <sessionId>]`
- `scroll <up|down|left|right> [pixels] [--target <target>] [--session <sessionId>]`
- `scrollintoview <target> [--session <sessionId>]`
- `wait <target|ms> [--text <text>] [--url <pattern>] [--load <state>] [--session <sessionId>]`
- `get <text|value|html|title|url> [target] [--session <sessionId>]`
- `find <role|text|label|placeholder|first|last|nth> ...`
- `upload <target> <file...> [--session <sessionId>]`
- `pdf <path> [--session <sessionId>]`
- `screenshot <path> [--annotate] [--press-escape] [--session <sessionId>]`
- `close [--session <sessionId>]`
- `sessions`
- `current`

## Operating Pattern

1. After token preflight, confirm profile strategy if the user did not specify it: existing profile or new/imported profile.
2. Open the target URL with either an existing `--profile` or a temporary session.
3. Capture `snapshot`.
4. Use the returned refs for deterministic actions.
5. After any mutating action, inspect whether refs may be stale and run `snapshot` again.
6. Use `current` or `sessions` if session state is unclear.
7. Save artifacts with `screenshot` or `pdf` when the task needs evidence or export.
8. End with `close`.

## Workflow Selection

- For profile warmup or repeated browsing: read [profile-warmup.md](./workflows/profile-warmup.md).
- For login plus cookie persistence: read [login-and-cookie-capture.md](./workflows/login-and-cookie-capture.md).
- For Reddit-specific persistent session routines: read [reddit-session-routine.md](./workflows/reddit-session-routine.md).
- For command resolution, environment setup, and common flags: read [command-map.md](./references/command-map.md).

## Output Expectations

- `snapshot` should be treated as the compact page model for the next step.
- Action commands should be read as session-state updates, not verbose logs.
- If a task depends on persistence, report the profile id and whether the session was closed cleanly.
