# Jira Skill for Muse

A Muse workspace skill that connects to Jira Cloud: search issues with
JQL, read issue details, create/update/comment/transition issues — all
from chat. Part of the [muse-atlassian-skill](../README.md) collection.

## What's in here

- `SKILL.md` — the skill definition (drop into `~/workspace/skills/jira/`)
- `bin/jira` — Python CLI wrapping the Jira Cloud REST API v3
- `INSTALL_PROMPT.md` — a prompt you paste to your Muse to install this
- `.gitignore` — keeps personal config out of the repo

## Install (via your Muse)

Paste the contents of `INSTALL_PROMPT.md` to your Muse and answer its
questions. It will install the skill, set up your API token securely,
and verify the connection.

## Install (manual)

1. Copy `SKILL.md` and `bin/` to `~/workspace/skills/jira/`.
2. Make the CLI executable: `chmod +x ~/workspace/skills/jira/bin/jira`.
3. Create your config (never commit this):
   ```bash
   mkdir -p ~/.config/jira-skill
   echo '{"host": "YOUR-SITE.atlassian.net"}' > ~/.config/jira-skill/config.json
   ```
4. Create a Jira API token at
   https://id.atlassian.com/manage-profile/security/api-tokens.
5. Ask your Muse to set up API access for provider `jira` (it will give
   you a secure link). When saving the credential, paste the full header
   value — including the word `Basic` and a space in front:
   ```
   Basic <base64 of "your-email:your-api-token">
   ```
   Generate it with: `echo -n "you@example.com:TOKEN" | base64`, then
   put `Basic ` in front. The `Basic ` prefix is required — without it
   Jira returns 401.
6. Verify: `~/workspace/skills/jira/bin/jira me`

## Usage

From chat, just ask: *"search my Jira for open bugs"*, *"show me
PROJ-123"*, *"comment on PROJ-123 saying fixed"*, etc.

Direct CLI:
```bash
bin/jira search --jql "project = PROJ AND status != Done" --max 25
bin/jira get PROJ-123
bin/jira create --project PROJ --summary "Fix login bug" --issuetype Bug
bin/jira comment PROJ-123 --body "Fixed in v2.1"
bin/jira transition PROJ-123 --to "In Progress"
```

## Privacy

Your Jira site URL, email, and API token never go in this repo. Config
lives in `~/.config/jira-skill/` (gitignored); the token lives in your
Muse Secure Vault. The skill code contains no personal data.
