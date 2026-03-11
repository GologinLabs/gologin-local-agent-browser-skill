# Login And Cookie Capture

Use this workflow when the user wants a persistent profile to log into a site and keep the resulting cookies/storage inside GoLogin.

## Recommended Flow

1. Require or reuse a persistent `--profile`.
2. Open the login URL.
3. Capture `snapshot -i`.
4. Use refs or semantic `find ...` commands to fill login fields.
5. Submit the form with `click`, `press Enter`, or a semantic `find` action.
6. Wait for post-login text, URL change, or page load.
7. Capture a fresh `snapshot`.
8. If needed, save a screenshot as evidence.
9. End with `close` so the SDK syncs the profile state.

## Example

```bash
gologin-local-agent-browser open https://example.com/login --profile profile_id --headless
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser type @e7 "username"
gologin-local-agent-browser type @e8 "password"
gologin-local-agent-browser press Enter
gologin-local-agent-browser wait --url "/dashboard"
gologin-local-agent-browser screenshot ./artifacts/login-proof.png
gologin-local-agent-browser close
```

## Cookie Persistence Notes

- The skill does not export cookies directly.
- Cookie persistence comes from using a real GoLogin profile and closing it cleanly.
- If the user needs the resulting logged-in state later, keep using the same profile id.

## Practical Tips

- Prefer `snapshot -i` on login pages so the important controls are visible in the compact view.
- If the page re-renders heavily after submit, expect stale refs and re-snapshot before continuing.
