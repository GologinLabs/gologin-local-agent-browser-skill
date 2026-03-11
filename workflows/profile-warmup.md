# Profile Warmup

Use this workflow when the goal is to make a persistent GoLogin profile browse a small set of real pages and accumulate normal session state.

## Recommended Flow

1. Reuse an existing `--profile`.
2. Open a relevant seed URL in headless mode unless visual debugging is needed.
3. Capture `snapshot`.
4. Perform a small number of realistic actions:
   - click one or two navigation links
   - scroll a few times
   - wait for visible text or page load
   - optionally open a second or third user-relevant page
5. Re-snapshot after page-changing actions.
6. End with `close`.

## Good Defaults

- Prefer 2-5 pages, not dozens.
- Prefer site-relevant pages over random traffic.
- Prefer a few coherent actions over noisy automation.
- Keep waits explicit with `wait` when the page is dynamic.

## Example

```bash
gologin-local-agent-browser open https://www.reddit.com --profile profile_id --headless
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser click @e5
gologin-local-agent-browser snapshot -i
gologin-local-agent-browser scroll down 700
gologin-local-agent-browser wait 1500
gologin-local-agent-browser close
```

## Avoid

- Reusing stale refs after navigation.
- Running aggressive loops without checking state.
- Leaving the profile open without a final `close`.
