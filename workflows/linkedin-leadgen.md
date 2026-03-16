# LinkedIn Leadgen

Use this workflow for SDR, recruiter, founder-led outbound, or candidate-sourcing profiles.

## Recommended Flow

1. Confirm whether the user wants an existing LinkedIn profile or a new one.
2. Confirm proxy mode before the first login.
3. Create or align the profile:

```bash
gologin-local-agent-browser profile-create "LinkedIn SDR 01" --template linkedin --proxy-country us
gologin-local-agent-browser doctor --use-case linkedin --check-proxy your_profile_id
```

4. Prefer a visible first run for login and checkpoint review:

```bash
gologin-local-agent-browser open https://www.linkedin.com --profile your_profile_id --visible
```

5. After login, warm with short repeated routes rather than a single marathon session.

## Notes

- One person or persona per profile is safer than reusing one profile across many operators.
- If the user has GoLogin traffic and no custom pool, `--proxy-country <cc>` is the fastest sane default.
- If extraction depends on full rendered DOM or repeated pagination, local GoLogin is preferred over stateless scraping.
