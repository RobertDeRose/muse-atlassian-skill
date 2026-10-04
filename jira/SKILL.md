---
name: "jira"
description: "Work with Jira Cloud: search issues with JQL, read issue details, create, update, comment on, and transition issues."
---

# Jira

## Purpose
Work with the user's Jira Cloud site: find issues, read details, create and
update issues, add comments, and move issues through workflows.

## Tooling
All commands go through `bin/jira`, a Python CLI that talks to the Jira
Cloud REST API v3 directly:

- `bin/jira me` — verify auth, show current user
- `bin/jira search --jql "project = PROJ AND status != Done" [--max 25]`
- `bin/jira get PROJ-123` — full details including description and comments
- `bin/jira create --project PROJ --summary "..." [--description "..." --issuetype Task --priority High --labels a,b]`
- `bin/jira update PROJ-123 [--summary ... --description ... --priority ... --labels ...]`
- `bin/jira comment PROJ-123 --body "..."`
- `bin/jira transition PROJ-123` — list available transitions
- `bin/jira transition PROJ-123 --to "In Progress"` — apply one

The CLI reads the site host from its own `HOST` constant and the API token
from the `custom.jira` connector via dynamic credential surrogates (see
Auth). It sends only `hsurr:*` surrogate values, only to the allowed host.

The official Atlassian CLI (`acli`, at `~/workspace/bin/acli`) is also
installed for terminal use and as a fallback for operations the wrapper
doesn't cover — but it manages its own auth separately and is not part of
the skill's credential flow.

## Auth
The API token is stored in the user's Secure Vault as the `custom.jira`
connector; nothing here collects one. Never ask the user to paste a raw
token in chat, set a secret environment variable, pass a secret flag, or
write an auth file.

The connector holds the full `Authorization` header value
(`Basic base64(email:api_token)`). The CLI attaches it via the surrogate
helpers in `/opt/hatch/skills/skill-creator/bin/dynamic_credentials.py`.

A 401 or 403 is a question about the request before it is a question about
the token. Check that the credential was attached at all: a request built
without the surrogate helpers carries nothing, and that looks exactly like
a wrong token. Only once a request that did carry the credential is still
rejected, use `credentials.request_api_access` with `reconnect: true` and
provider `jira` to replace it.

## Operating Rules
1. Use this skill when the user asks about Jira, their tickets, or issues.
2. Never print, log, or persist the API token or the Authorization header.
3. Confirm before creating, updating, commenting, transitioning, or deleting
   anything — reads are free, writes need the user's go-ahead.
4. Jira Cloud text fields use Atlassian Document Format; the wrapper handles
   the conversion from plain text.
5. Keep JQL in the user's words; don't invent project keys — `search` with
   broad JQL first if unsure.
