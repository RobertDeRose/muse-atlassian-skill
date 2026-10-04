# Muse Atlassian Skill

A collection of Muse workspace skills for Atlassian Cloud services.
Each service lives in its own directory with a copy-paste install prompt
you can send to your Muse.

## Services

- **jira/** — Jira Cloud: search issues with JQL, read details, create,
  update, comment, and transition issues from chat.
- **confluence/** — Confluence Cloud: search pages with CQL, read
  content, create and update pages from chat.

## Install

Each service directory has its own `INSTALL_PROMPT.md`. Paste it to your
Muse and answer its questions — it will install the skill, set up your
API token securely, and verify the connection.

## Privacy

Your Atlassian site URL, email, and API tokens never go in this repo.
Config lives in your local `~/.config/` (gitignored); tokens live in
your Muse Secure Vault. The skill code contains no personal data.
