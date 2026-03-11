# Tool Contracts

This skill wraps the `gologin-local-agent-browser` CLI and its daemon-backed local GoLogin profile model.

## Summary

| Skill tool | CLI command | Requires | Returns |
| --- | --- | --- | --- |
| `local_browser_open` | `open` | `GOLOGIN_API_TOKEN` | Session summary |
| `local_browser_snapshot` | `snapshot` | `GOLOGIN_API_TOKEN` | Snapshot with refs |
| `local_browser_click` | `click` | `GOLOGIN_API_TOKEN` | Action status |
| `local_browser_type` | `type` | `GOLOGIN_API_TOKEN` | Action status |
| `local_browser_run` | `run` | `GOLOGIN_API_TOKEN` | Runbook job record |
| `local_browser_batch` | `batch` | `GOLOGIN_API_TOKEN` | Batch job record |
| `local_browser_jobs` | `jobs` | none beyond local state | Job list |
| `local_browser_job` | `job` | none beyond local state | Job detail |
| `local_browser_screenshot` | `screenshot` | `GOLOGIN_API_TOKEN` | Screenshot path |
| `local_browser_pdf` | `pdf` | `GOLOGIN_API_TOKEN` | PDF path |
| `local_browser_profiles` | `profiles` | `GOLOGIN_API_TOKEN` | Local registry list |
| `local_browser_profile_create` | `profile-create` | `GOLOGIN_API_TOKEN` | Created profile summary |

## local_browser_open

```bash
gologin-local-agent-browser open "https://example.com" --profile your_profile_id
```

Use when:
A local GoLogin profile session should begin.

## local_browser_snapshot

```bash
gologin-local-agent-browser snapshot -i
```

Use when:
The next deterministic ref is needed.

## local_browser_click

```bash
gologin-local-agent-browser click @e3
```

Use when:
A ref or selector should be clicked.

## local_browser_type

```bash
gologin-local-agent-browser type @e2 "hello world"
```

Use when:
Text should be typed into a target.

## local_browser_run

```bash
gologin-local-agent-browser run ./examples/runbook-warmup.json --profile your_profile_id --name warmup
```

Use when:
One reusable scenario should run against one session or profile.

## local_browser_batch

```bash
gologin-local-agent-browser batch ./examples/runbook-warmup.json --targets ./targets.json --name batch-warmup
```

Use when:
One runbook should be replayed across multiple targets.

## local_browser_jobs

```bash
gologin-local-agent-browser jobs --limit 10
```

Use when:
Recent run or batch history should be inspected.

## local_browser_job

```bash
gologin-local-agent-browser job job-20260311123000-ab12cd34 --json
```

Use when:
One stored execution should be inspected in detail.

## local_browser_profiles

```bash
gologin-local-agent-browser profiles --platform reddit
```

Use when:
The local profile registry should be queried.

## local_browser_profile_create

```bash
gologin-local-agent-browser profile-create "reddit-main" --platform reddit --account user1
```

Use when:
A new persistent GoLogin profile should be created and tracked.
