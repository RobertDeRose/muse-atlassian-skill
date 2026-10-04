# Install prompt — paste this to your Muse

> I want to install the Jira skill for Muse from this repo
> (https://github.com/RobertDeRose/muse-atlassian-skill). Please:
>
> 1. Copy `jira/SKILL.md` and `jira/bin/` into `~/workspace/skills/jira/`
>    and make `bin/jira` executable.
> 2. Ask me for my Jira Cloud site host (the `xxx.atlassian.net` part)
>    and save it to `~/.config/jira-skill/config.json` as
>    `{"host": "..."}`. Never put my host, email, or token in the repo
>    or any committed file.
> 3. Walk me through creating a Jira API token at
>    id.atlassian.com → Security → API tokens.
> 4. Set up API access for provider `jira` using your
>    `credentials.request_api_access` tool, with:
>    - `api_hosts`: my Jira host from step 2
>    - `auth_scheme`: `api_key`
>    - `placement`: `custom_header:Authorization`
>    Then give me the secure link. Tell me to paste the full header
>    value `Basic <base64 of "my-email:my-api-token">` — including the
>    word `Basic` and a space in front. I generate the base64 with
>    `echo -n "email:token" | base64`.
> 5. Once I have saved it, verify the connection by running
>    `~/workspace/skills/jira/bin/jira me` — it should print my Jira
>    user. If it 401s, help me check the credential format (especially
>    the `Basic ` prefix) before anything else.
> 6. Confirm the skill is ready and show me an example JQL search.
