---
name: "confluence"
description: "Work with Confluence Cloud: search pages with CQL, read page content, create and update pages."
---

# Confluence

## Purpose
Work with the user's Confluence Cloud site: find pages, read content,
create new pages, and update existing ones.

## Tooling
All commands go through `bin/confluence`, a Python CLI that talks to the
Confluence Cloud REST API directly:

- `bin/confluence me` — verify auth, show current user
- `bin/confluence search --cql "text ~ \"deploy\" AND type = page" [--max 25]`
- `bin/confluence get 123456` — page details with a body preview
- `bin/confluence create --space ENG --title "Runbook" --body "<p>...</p>" [--parent 123456]`
- `bin/confluence update 123456 --body "<p>...</p>" [--title "New title"]`

Page bodies use Confluence storage format (XHTML). Plain `<p>...</p>`
markup works for simple content.

The CLI reads the site host from `~/.config/confluence-skill/config.json`
and the API token from the `custom.confluence` connector via dynamic
credential surrogates (see Auth). It sends only `hsurr:*` surrogate
values, only to the allowed host.

## Auth
The API token is stored in the user's Secure Vault as the
`custom.confluence` connector; nothing here collects one. Never ask the
user to paste a raw token in chat, set a secret environment variable,
pass a secret flag, or write an auth file.

Atlassian API tokens are account-level: the same token used for Jira
works for Confluence. The connector holds the full `Authorization`
header value (`Basic base64(email:api_token)`) — including the `Basic `
prefix, which is required. The CLI attaches it via the surrogate helpers
in `/opt/hatch/skills/skill-creator/bin/dynamic_credentials.py`.

A 401 or 403 is a question about the request before it is a question
about the token. Check that the credential was attached at all and that
the `Basic ` prefix is present. Only once a request that did carry the
credential is still rejected, use `credentials.request_api_access` with
`reconnect: true` and provider `confluence` to replace it.

## Operating Rules
1. Use this skill when the user asks about Confluence, wiki pages, or docs.
2. Never print, log, or persist the API token or the Authorization header.
3. Confirm before creating or updating anything — reads are free, writes
   need the user's go-ahead.
4. Keep CQL in the user's words; `search` broadly first if unsure of
   space keys.
