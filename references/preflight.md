# Preflight Routing

Use this checklist before touching a local GoLogin profile.

## Four Blocking Questions

1. Which use case is this?
   - `linkedin`
   - `ads`
   - `smm`
   - `scraping`
   - `geo`
   - something else
2. Should the task use an existing profile or create/import a new one?
3. What is the proxy plan?
   - no proxy
   - GoLogin proxy by country
   - custom proxy
4. Should the first run be visible for review, or unattended in the background?

## Mapping

| Use case | CLI defaults |
| --- | --- |
| `linkedin` | `profile-create --template linkedin`, then `doctor --use-case linkedin --check-proxy <profileId>` |
| `ads` or `facebook` | `profile-create --template ads`, then `doctor --use-case ads --check-proxy <profileId>` |
| `smm` | `profile-create --template smm`, then `doctor --use-case smm --check-proxy <profileId>` |
| `scraping` | `profile-create --template scraping`, then `doctor --use-case scraping` |
| `geo` | `profile-create --template geo --proxy-country <cc>`, then `doctor --use-case geo --check-proxy <profileId>` |

## Rules

- Do not create or mutate a profile until profile strategy and proxy strategy are explicit.
- For `linkedin`, `ads`, and `geo`, treat proxy choice as mandatory preflight.
- For first login, manual checkpoint, or warmup review, prefer `--visible`.
- For repeated unattended runs, prefer `--headless` or `--background`.
- If the user already has the right remote profile, prefer `profiles --remote` plus `profile-import` or direct `--profile` over creating another one.
