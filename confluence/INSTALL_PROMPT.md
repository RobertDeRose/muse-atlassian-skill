# Install prompt — paste this to your Muse

> I want to install the Confluence skill for Muse from this repo
> (https://github.com/RobertDeRose/muse-atlassian-skill). Please:
>
> 1. Copy `confluence/SKILL.md` and `confluence/bin/` into
>    `~/workspace/skills/confluence/` and make `bin/confluence`
>    executable.
> 2. Ask me for my Confluence Cloud site host (the `xxx.atlassian.net`
>    part — usually the same as my Jira host) and save it to
>    `~/.config/confluence-skill/config.json` as `{"host": "..."}`.
>    Never put my host, email, or token in the repo or any committed file.
> 3. Walk me through creating an Atlassian API token at
>    id.atlassian.com → Security → API tokens (tokens are account-level,
>    so I can reuse my Jira token).
> 4. Set up API access for provider `confluence` using your
>    `credentials.request_api_access` tool, with:
>    - `api_hosts`: my Confluence host from step 2
>    - `auth_scheme`: `api_key`
>    - `placement`: `custom_header:Authorization`
>    Then give me the secure link. Tell me to paste the full header
>    value `Basic <base64 of "my-email:my-api-token">` — including the
>    word `Basic` and a space in front. I generate the base64 with
>    `echo -n "email:token" | base64`.
> 5. Once I've saved it, verify the connection by running
>    `~/workspace/skills/confluence/bin/confluence me` — it should print
>    my Confluence user. If it 401s, help me check the credential format
>    (especially the `Basic ` prefix) before anything else.
> 6. Confirm the skill is ready and show me an example CQL search.
