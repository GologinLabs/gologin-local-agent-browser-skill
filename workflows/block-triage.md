# Block Triage

Use this when the user reports captcha, bans, forced logouts, session loss, or unstable warmup.

## First Checks

1. Confirm the use case and target site.
2. Run:

```bash
gologin-local-agent-browser doctor --use-case linkedin --check-proxy your_profile_id
```

Replace the use case as needed.

3. Inspect whether the problem is:
   - proxy mismatch
   - no proxy on a geo-sensitive account
   - lost browser session
   - challenge or captcha page
   - profile already busy

## Response Pattern

- If proxy drift is visible, fix proxy strategy before any more browsing.
- If the site shows a challenge page, skip the target or end the cycle cleanly instead of repeatedly re-opening the profile.
- If the session was lost, reopen once through the CLI and keep using the same profile rather than spawning parallel local browsers.
- If the target needs more rendered DOM or repeated navigation, stay in local GoLogin instead of switching to stateless scraping.
