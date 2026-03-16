# Facebook Ads And SMM

Use this workflow when the user is operating ad accounts, Meta business assets, or shared social profiles.

## Ads Operators

```bash
gologin-local-agent-browser profile-create "FB Buyer 01" --template ads --proxy-country us
gologin-local-agent-browser doctor --use-case ads --check-proxy your_profile_id
gologin-local-agent-browser open https://business.facebook.com --profile your_profile_id --visible
```

Rules:

- Prefer one profile per buyer or business identity.
- Do not patch proxy settings after login unless the user explicitly wants that risk.
- Use visible mode for first login or payment-review flows.

## SMM Shared Access

```bash
gologin-local-agent-browser profile-create "Brand SMM" --template smm --proxy-country gb
gologin-local-agent-browser doctor --use-case smm --check-proxy your_profile_id
```

Rules:

- Keep notes and tags current so another operator can inherit the profile safely.
- Prefer cookie or storage export only when a real handoff is needed.
- Use `tabs`, `cookies`, `storage-export`, and `storage-import` instead of ad-hoc browser hacks.
