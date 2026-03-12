# Tool Contracts

This skill wraps the `gologin-local-agent-browser` CLI and its daemon-backed local GoLogin profile model.

## Summary

| Skill tool | CLI command | Requires | Returns |
| --- | --- | --- | --- |
| `local_browser_doctor` | `doctor` | local install, token strongly preferred for runtime checks | Daemon and config diagnostics |
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
| `local_browser_profiles` | `profiles` | `GOLOGIN_API_TOKEN` | Remote, local, or combined profile list |
| `local_browser_profile` | `profile` | `GOLOGIN_API_TOKEN` | One profile detail |
| `local_browser_profile_create` | `profile-create` | `GOLOGIN_API_TOKEN` | Created profile summary |
| `local_browser_profile_import` | `profile-import` | `GOLOGIN_API_TOKEN` | Imported registry record |
| `local_browser_profile_update` | `profile-update` | `GOLOGIN_API_TOKEN` | Updated registry record |
| `local_browser_profile_sync` | `profile-sync` | `GOLOGIN_API_TOKEN` | Synced local and remote summary |
| `local_browser_profile_delete` | `profile-delete` | `GOLOGIN_API_TOKEN` | Delete result |
| `local_browser_tabs` | `tabs` | `GOLOGIN_API_TOKEN` | Current tab inventory |
| `local_browser_cookies` | `cookies` | `GOLOGIN_API_TOKEN` | Cookies JSON or export path |
| `local_browser_storage` | `storage-export` | `GOLOGIN_API_TOKEN` | Local/session storage dump |
| `local_browser_eval` | `eval` | `GOLOGIN_API_TOKEN` | Evaluated JavaScript result |

## local_browser_doctor

```bash
gologin-local-agent-browser doctor --json
```

Use when:
Daemon reachability, config resolution, or local CLI health must be inspected before guessing.

## local_browser_open

```bash
gologin-local-agent-browser open "https://example.com" --profile your_profile_id --visible
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
gologin-local-agent-browser profiles --remote --platform reddit --json
```

Use when:
Existing GoLogin profiles or local registry entries should be queried. Use `--remote`, `--local`, or `--all` explicitly when the source matters.

## local_browser_profile

```bash
gologin-local-agent-browser profile 69b05c486375d47ab0f12630 --remote --json
```

Use when:
One profile should be inspected in detail.

## local_browser_profile_create

```bash
gologin-local-agent-browser profile-create "reddit-main" --platform reddit --account user1
```

Use when:
A new persistent GoLogin profile should be created and tracked.

## local_browser_profile_import

```bash
gologin-local-agent-browser profile-import 69b05c486375d47ab0f12630 --platform reddit --account user1
```

Use when:
An existing remote GoLogin profile should be added to the local registry with metadata.

## local_browser_profile_update

```bash
gologin-local-agent-browser profile-update 69b05c486375d47ab0f12630 --notes "Warmup finished" --add-tags warmed
```

Use when:
Local profile metadata should be edited.

## local_browser_profile_sync

```bash
gologin-local-agent-browser profile-sync 69b05c486375d47ab0f12630 --json
```

Use when:
The local registry and remote GoLogin profile should be synchronized.

## local_browser_profile_delete

```bash
gologin-local-agent-browser profile-delete 69b05c486375d47ab0f12630 --remote
```

Use when:
A local registry entry or remote profile should be deleted.

## local_browser_tabs

```bash
gologin-local-agent-browser tabs --session s1
```

Use when:
Multiple tabs may be open and the session needs inspection.

## local_browser_cookies

```bash
gologin-local-agent-browser cookies --session s1 --output ./cookies.json
```

Use when:
Cookies should be inspected, exported, imported, or cleared.

## local_browser_storage

```bash
gologin-local-agent-browser storage-export ./storage.json --scope both --session s1
```

Use when:
Local storage or session storage should be exported, imported, or cleared.

## local_browser_eval

```bash
gologin-local-agent-browser eval "document.title" --json --session s1
```

Use when:
The current page state should be inspected or a small JavaScript expression should run in-browser.
