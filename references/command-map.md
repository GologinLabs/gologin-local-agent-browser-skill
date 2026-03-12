# Command Map

## Preferred CLI

Use the globally installed command:

```bash
gologin-local-agent-browser <command> ...
```

If it is not installed yet:

```bash
npm install -g gologin-local-agent-browser-cli
```

If you want to run from a source checkout:

```bash
git clone https://github.com/GologinLabs/gologin-local-agent-browser.git
cd gologin-local-agent-browser
npm install
npm run build
node ./dist/cli.js <command> ...
```

## Blocking Preflight

- Before any runtime command, confirm that `GOLOGIN_TOKEN` or `GOLOGIN_API_TOKEN` is available.
- If the token is missing, stop and ask the user for it.
- Do not probe `profiles`, `sessions`, `current`, daemon state, local config files, Orbita paths, or CLI `--help` as a workaround for a missing token.
- Use `doctor` only when the user is explicitly debugging install or daemon behavior, or when token preflight has already been satisfied.
- `GOLOGIN_PROFILE_ID` is optional. Only after a token exists may the skill list profiles, inspect registry state, or create/import a profile.
- If the user did not specify profile strategy, stop and ask one explicit question first: use an existing profile, or create/import a new profile.
- Do not auto-list profiles or auto-create a profile until that choice is clear.

## Environment

- `GOLOGIN_TOKEN` or `GOLOGIN_API_TOKEN`: required
- `GOLOGIN_PROFILE_ID`: optional default profile
- `GOLOGIN_HEADLESS=true`: useful for unattended automation
- `GOLOGIN_EXECUTABLE_PATH`: only when Orbita must be forced to a custom binary
- `GOLOGIN_TMPDIR`: only when temp profile data should be stored elsewhere

## Open Patterns

Persistent profile:

```bash
gologin-local-agent-browser open https://example.com --profile profile_id --headless
```

Temporary profile:

```bash
gologin-local-agent-browser open https://example.com --headless
```

Temporary profile with Gologin proxy:

```bash
gologin-local-agent-browser open https://example.com --proxy-country us --headless
```

Temporary profile with custom proxy:

```bash
gologin-local-agent-browser open https://example.com \
  --proxy-host 1.2.3.4 \
  --proxy-port 8080 \
  --proxy-mode http \
  --proxy-user user \
  --proxy-pass secret \
  --headless
```

## Session Inspection

- `doctor` to inspect daemon, config, and transport health
- `profiles --remote` to inspect remote GoLogin profiles
- `profiles --local` to inspect only the local registry
- `profiles --all` to combine both sources
- `profile <id> --remote` or `profile <id> --local` for one profile detail
- `snapshot` to get current actionable refs
- `current` to inspect active session metadata
- `sessions` to list all daemon-held sessions
- `tabs` to inspect multi-tab state
- `cookies` to export or inspect browser cookies
- `storage-export` to inspect local/session storage
- `eval` for small in-page inspections
- `jobs` and `job` to inspect stored run or batch history

## Profile Management

- `profile-create` for a new persistent GoLogin profile plus registry entry
- `profile-import` to bring an existing remote profile into the local registry
- `profile-update` to edit local metadata such as platform, account label, notes, and tags
- `profile-sync` to synchronize local metadata with the remote GoLogin profile
- `profile-delete [--remote]` to remove a local entry or also delete the remote profile

## Persistence Rules

- Use `--profile` for anything that should retain cookies, local storage, and browsing state.
- End with `close` so the GoLogin SDK posts profile state back to storage.
- Temporary profiles are disposable. Persistent profiles are for real warmup/login/account workflows.

## Visibility Modes

- `--headless` and `--background` are preferred for unattended runs.
- `--headed` and `--visible` are preferred when the user wants to watch the profile, review warmup, or debug a login flow.

## Ref Discipline

- Use refs exactly as returned, for example `@e4`.
- After navigation, page refresh, login submit, modal changes, or DOM-heavy interactions, run `snapshot` again.
- If a command says the snapshot is stale, stop using old refs immediately.
