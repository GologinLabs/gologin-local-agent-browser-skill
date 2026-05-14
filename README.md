# Gologin Local Agent Browser Skill

This standalone skill is deprecated. Local Orbita automation is now part of the unified [Gologin Agent Browser Skill](https://github.com/GologinLabs/gologin-agent-browser-skill) and [gologin-agent-browser-cli](https://github.com/GologinLabs/agent-browser).

## Install The Current Skill

Use the unified skill:

```bash
npx skills add GologinLabs/gologin-agent-browser-skill
```

Install the unified CLI:

```bash
npm install -g gologin-agent-browser-cli
```

## Local Runtime Commands

Use `--runtime local` for local Orbita profile automation:

```bash
export GOLOGIN_TOKEN="your_gologin_token"

gologin-agent-browser --runtime local doctor --json
gologin-agent-browser --runtime local profiles --remote --json
gologin-agent-browser --runtime local open https://example.com --profile your_profile_id
gologin-agent-browser --runtime local snapshot -i
gologin-agent-browser --runtime local close
```

The old command remains as a compatibility alias when installed from `gologin-agent-browser-cli@0.3.0+`:

```bash
gologin-local-agent-browser <command> ...
```

Do not install the deprecated `gologin-local-agent-browser-cli` package for new work.

## Why This Repo Still Exists

Older prompts and installed agents may still refer to `gologin-local-agent-browser-skill`. This repo now redirects them to the unified browser skill instead of teaching a separate product boundary.
