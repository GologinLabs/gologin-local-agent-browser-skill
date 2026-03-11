# Reddit Session Routine

Use this workflow for a persistent Reddit profile where the user wants login, light warmup, reading, or controlled interaction inside an existing GoLogin profile.

## Recommended Flow

1. Reuse a persistent `--profile`.
2. Open the specific Reddit URL the task starts from.
3. Capture `snapshot -i`.
4. If login is required, complete it first and re-snapshot.
5. For reading or warmup:
   - scroll feeds or threads
   - open one or two posts
   - wait between major actions
6. For posting or account actions:
   - prefer explicit refs from the latest snapshot
   - re-snapshot after opening composers, menus, or modal dialogs
7. End with `close`.

## Example

```bash
gologin-local-agent-browser open https://www.reddit.com/login --profile profile_id --headless
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser type @e7 "username"
gologin-local-agent-browser type @e8 "password"
gologin-local-agent-browser press Enter
gologin-local-agent-browser wait --text "Home"
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser click @e12
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser scroll down 900
gologin-local-agent-browser wait 1200
gologin-local-agent-browser close
```

## Notes

- Reddit pages are dynamic, so stale refs are common after login, feed updates, or composer opens.
- If the task becomes multi-step and stateful, inspect `current` between phases.
- For evidence or review, save screenshots at meaningful checkpoints instead of after every step.
