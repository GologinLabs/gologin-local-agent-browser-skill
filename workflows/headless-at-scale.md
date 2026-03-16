# Headless At Scale

Use this pattern for 10, 50, or 100 short unattended local runs.

## Recipe

1. Keep each runbook route short and coherent.
2. Reuse one profile per account identity.
3. Verify proxy alignment before the batch:

```bash
gologin-local-agent-browser doctor --use-case scraping --check-proxy your_profile_id
```

4. Run batched jobs with conservative concurrency:

```bash
gologin-local-agent-browser batch ./examples/runbook-warmup.json \
  --targets ./examples/batch-targets.json \
  --concurrency 5 \
  --name short-warmup
```

5. For one profile over time, prefer repeated short `run` cycles:

```bash
gologin-local-agent-browser run ./examples/runbook-warmup.json \
  --profile your_profile_id \
  --repeat 6 \
  --pause-min-ms 30000 \
  --pause-max-ms 120000
```

## Rules

- Prefer `--headless` or `--background` after the first visible checkpoint is clean.
- Do not create one giant deterministic route just to reach a long duration target.
- When failure hits one step, prefer cycle-level continuation and partial success over throwing away the entire campaign.
