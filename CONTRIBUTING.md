# Contributing a Skill

Skills are community-contributed. If you've built something useful for 0x01 agents, submit it here and it becomes installable by anyone from the marketplace.

## Quick start

1. **Fork** this repo
2. **Create** `skills/<your-skill-name>/SKILL.toml`
3. **Test** it locally (see below)
4. **Open a PR** — title: `skill: add <name>`

That's it. No build step, no CI setup needed.

## Naming rules

- Lowercase letters, digits, hyphens, underscores only
- No dots, slashes, or spaces
- Must be unique in the marketplace
- Good: `solana-nft`, `price_alert`, `coingecko`
- Bad: `My Skill`, `../../../etc`, `trade` (taken)

## What makes a good skill

**Passes the usefulness test**
- Does one thing well. Broad "do everything" skills are hard to maintain.
- Has clear instructions in `prompts` — the agent needs to know *when* to use each tool.

**Safe commands**
- Only HTTPS for external calls
- No hardcoded API keys or secrets
- No shell injection risks (use `jq` for JSON building, not string interpolation)
- Use `${ZX01_NODE:-http://127.0.0.1:9090}` + `Authorization: Bearer ${ZX01_TOKEN:-}` for node API access

**Well-described tools**
- `description` explains what the tool returns, not just what it does
- `[tools.args]` has a description for every `{placeholder}` in the command
- Parameter names are obvious (`token_mint` not `t`, `query` not `q`)

## Testing locally

1. Write your `SKILL.toml` to the zeroclaw skill workspace:
   ```
   # on Android (via adb shell or node API):
   POST /skill/write
   { "name": "my-skill", "content_b64": "<base64 of your SKILL.toml>" }
   ```
2. Tell your agent: `reload skills`
3. Verify the tools appear and the prompt injection works

Or ask your agent to write and install the skill directly from chat.

## PR checklist

- [ ] `skills/<name>/SKILL.toml` is the only file changed (one skill per PR)
- [ ] `name` in TOML matches the directory name
- [ ] `author` is set to your GitHub handle
- [ ] All tool commands use HTTPS for external calls
- [ ] No secrets hardcoded in commands
- [ ] `prompts` explains when to use each tool (not just what the tools are)
- [ ] Tool `description` fields are clear enough for an LLM to decide when to call them

## PR title format

```
skill: add <name>
```

Examples:
- `skill: add coingecko`
- `skill: add solana-nft`
- `skill: update trade v1.2.0`

## After merge

Your skill becomes immediately available via:
- `skill_marketplace_install <name>` (from agent chat)
- The marketplace at [skills.0x01.world](https://skills.0x01.world)
- Direct URL: `https://skills.0x01.world/skills/<name>/SKILL.toml`

## Updating an existing skill

Open a PR with the updated `SKILL.toml`. Bump the `version` field. Include a brief changelog in the PR description.

## Questions

Open an issue or ask in the 0x01 community.
