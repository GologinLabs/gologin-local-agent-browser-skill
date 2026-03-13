# Profile Warmup

Use this workflow when the goal is to make a persistent GoLogin profile browse a small set of real pages and accumulate normal session state.

Treat warmup as a campaign of short coherent sessions, not one giant deterministic run.

## Recommended Flow

1. Reuse an existing `--profile`.
2. If the workflow needs a new profile instead, decide the proxy mode before creating it:
   - no proxy
   - GoLogin proxy by country with `--proxy-country <cc>`
   - custom proxy with host/port credentials
3. Encode one coherent route in a runbook.
4. Run that route for multiple cycles with `run --repeat` or `run --duration-ms`, plus pauses between cycles.
   Repeated campaigns now default to a warmup-friendly error policy in the CLI: one failed site or one detected challenge should mark the cycle as partial instead of killing the whole campaign immediately.
5. Inside the runbook, open a relevant seed URL in headless mode unless visual debugging is needed.
6. Capture `snapshot`.
7. Perform a small number of realistic actions:
   - click one or two navigation links
   - scroll a few times
   - wait for visible text or page load
   - optionally open a second or third user-relevant page
8. Re-snapshot after page-changing actions.
9. End each cycle with `close`.

## Good Defaults

- Prefer 2-5 pages, not dozens.
- Prefer site-relevant pages over random traffic.
- Prefer a few coherent actions over noisy automation.
- Prefer a small multi-tab route for warmup-heavy sessions:
  - first `open` one relevant page
  - then `tabopen` one or two related pages
  - then `tabfocus` and interact across those tabs instead of replacing the same page over and over
- Keep waits explicit with `wait` when the page is dynamic.
- Prefer `minDelayMs` and `maxDelayMs` on runbook steps instead of the exact same wait every time.
- Prefer `retry` and `retryBackoffMs` on fragile steps such as clicks after navigation.
- For multi-hour warmup, use many short cycles with pauses, not one multi-hour runbook.

## Example Runbook

```json
{
  "steps": [
    {
      "command": "open",
      "args": ["https://www.reddit.com"],
      "flags": { "headless": true },
      "minDelayMs": 800,
      "maxDelayMs": 1800
    },
    {
      "command": "snapshot",
      "flags": { "interactive": true },
      "minDelayMs": 600,
      "maxDelayMs": 1400
    },
    {
      "command": "tabopen",
      "args": ["https://www.reddit.com/r/Porsche/"],
      "minDelayMs": 700,
      "maxDelayMs": 1600
    },
    {
      "command": "tabfocus",
      "args": [1],
      "minDelayMs": 600,
      "maxDelayMs": 1200
    },
    {
      "command": "scroll",
      "args": ["down", 700],
      "minDelayMs": 700,
      "maxDelayMs": 1500
    },
    {
      "command": "snapshot",
      "flags": { "interactive": true },
      "minDelayMs": 600,
      "maxDelayMs": 1400
    },
    {
      "command": "tabfocus",
      "args": [0],
      "minDelayMs": 900,
      "maxDelayMs": 2200
    },
    {
      "command": "wait",
      "args": [1500]
    },
    {
      "command": "close",
      "minDelayMs": 500,
      "maxDelayMs": 1200
    }
  ]
}
```

## Example Campaign

```bash
gologin-local-agent-browser run ./warmup-route.json \
  --profile profile_id \
  --repeat 12 \
  --pause-min-ms 45000 \
  --pause-max-ms 120000 \
  --name warmup-campaign
```

## Avoid

- Reusing stale refs after navigation.
- Running one giant deterministic session for hours.
- Leaving the profile open without a final `close`.
