---
name: gologin-local-agent-browser-skill
description: Deprecated compatibility skill. Local GoLogin Orbita automation now belongs to gologin-agent-browser-skill and the unified gologin-agent-browser-cli. Use this only as a redirect when older prompts mention gologin-local-agent-browser, Orbita, profile warmup, local profile cookies, or local GoLogin profile automation.
---

# GoLogin Local Agent Browser Skill

This skill is deprecated as a standalone entrypoint. Local Orbita support has moved into the unified `gologin-agent-browser-cli` package.

Use the main `gologin-agent-browser-skill` rules and run local work through:

```bash
gologin-agent-browser --runtime local <command> ...
```

Compatibility alias:

```bash
gologin-local-agent-browser <command> ...
```

The alias is provided by `gologin-agent-browser-cli@0.3.0+`, not by the old `gologin-local-agent-browser-cli` package.

## Routing

- Use `--runtime local` for local Orbita profile work, warmup, native OS alignment, existing local cookies, profile runbooks, and local profile metadata commands.
- Use `--runtime cloud` or default cloud runtime for remote Cloud Browser sessions and cloud parallelism.
- Use `gologin-web-access-skill` for scrape-first reading, extraction, mapping, crawling, or monitoring.

## Canonical Commands

```bash
gologin-agent-browser --runtime local profile-create "reddit-main" --template smm --proxy-country us
gologin-agent-browser --runtime local open https://example.com --profile your_profile_id
gologin-agent-browser --runtime local snapshot
gologin-agent-browser --runtime local close
gologin-agent-browser --runtime local run ./examples/runbook-warmup.json --profile your_profile_id
```

Do not call the raw `gologin` SDK directly for local-profile work. The unified CLI owns daemon startup, Orbita executable diagnostics, profile/proxy handling, snapshots, refs, runbooks, and job history.
