# Confluence Skill for Muse

A Muse workspace skill that connects to Confluence Cloud: search pages
with CQL, read content, create and update pages — all from chat. Part of
the [muse-atlassian-skill](../README.md) collection.

## What's in here

- `SKILL.md` — the skill definition (drop into `~/workspace/skills/confluence/`)
- `bin/confluence` — Python CLI wrapping the Confluence Cloud REST API
- `INSTALL_PROMPT.md` — a prompt you paste to your Muse to install this
- `.gitignore` — keeps personal config out of the repo

## Install (via your Muse)

Paste the contents of `INSTALL_PROMPT.md` to your Muse and answer its
questions. It will install the skill, set up your API token securely,
and verify the connection.

## Install (manual)

1. Copy `SKILL.md` and `bin/` to `~/workspace/skills/confluence/`.
2. Make the CLI executable: `chmod +x ~/workspace/skills/confluence/bin/confluence`.
3. Create your config (never commit this):
   ```bash
   mkdir -p ~/.config/confluence-skill
   echo '{"host": "YOUR-SITE.atlassian.net"}' > ~/.config/confluence-skill/config.json
   ```
4. Create an Atlassian API token at
   https://id.atlassian.com/manage-profile/security/api-tokens
   (tokens are account-level — the same one you use for Jira works here).
5. Ask your Muse to set up API access for provider `confluence` (it will
   give you a secure link). When saving the credential, paste the full
   header value — including the word `Basic` and a space in front:
   ```
   Basic <base64 of "your-email:your-api-token">
   ```
   Generate it with: `echo -n "you@example.com:TOKEN" | base64`, then
   put `Basic ` in front. The `Basic ` prefix is required — without it
   Confluence returns 401.
6. Verify: `~/workspace/skills/confluence/bin/confluence me`

## Usage

From chat, just ask: *"search Confluence for the deploy runbook"*,
*"show me that architecture page"*, etc.

Direct CLI:
```bash
bin/confluence search --cql "text ~ \"deploy\" AND type = page" --max 10
bin/confluence get 123456
bin/confluence create --space ENG --title "Runbook" --body "<p>Steps...</p>"
bin/confluence update 123456 --body "<p>Updated steps...</p>"
```

## Privacy

Your Confluence site URL, email, and API token never go in this repo.
Config lives in `~/.config/confluence-skill/` (gitignored); the token
lives in your Muse Secure Vault. The skill code contains no personal data.
