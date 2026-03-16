# Migrations

Use this when the user is moving from Multilogin, Dolphin, AdsPower, or ad-hoc scripts.

## Migration Pattern

1. Identify the current working unit:
   - one browser profile
   - one ad account
   - one operator identity
   - one scraping route
2. Create or import the matching GoLogin profile.
3. Stamp the workflow with a local template:

```bash
gologin-local-agent-browser profile-import existing_remote_profile_id
gologin-local-agent-browser profile-update existing_remote_profile_id --template ads
```

4. Verify local runtime and proxy alignment:

```bash
gologin-local-agent-browser doctor --use-case ads --check-proxy existing_remote_profile_id
```

## Mapping Heuristics

- Multilogin or Dolphin profile used for outreach -> `--template linkedin`
- AdsPower or browser profile used for media buying -> `--template ads`
- Shared social profile -> `--template smm`
- Rendered cookie-backed automation -> `--template scraping`
- Local-market or geo QA profile -> `--template geo`

## Rule

Do not re-create the old stack with raw SDK scripts first. Move the workflow into the CLI surface, then automate on top of the CLI.
