# 0x01 Skills Marketplace

**[skills.0x01.world](https://skills.0x01.world)** — Extend your 0x01 agent without an app update.

A skill is a single `SKILL.toml` file that injects instructions into your agent's system prompt and registers shell tools it can call. Skills are hot-loaded — install from the app or by telling your agent to install one.

## Bundled skills

| Skill | Description | Requires node |
|-------|-------------|:---:|
| `bags` | Launch and trade tokens on Bags.fm | ✓ |
| `trade` | Jupiter swaps, limit orders, DCA | ✓ |
| `launchlab` | Raydium LaunchLab bonding-curve buy/sell | ✓ |
| `cpmm` | Create a Raydium CPMM liquidity pool | ✓ |
| `health` | Android Health Connect — sleep, HR, HRV, steps | ✓ |
| `skill-manager` | Install/remove skills from chat | pre-installed |
| `github` | Search repos, read READMEs, browse issues | — |
| `hn-news` | Hacker News — top stories, comments | — |
| `web-search` | DuckDuckGo web search | — |
| `weather` | Current weather and forecasts | — |

## Install a skill

**From the app** — My → Skills → paste the raw SKILL.toml URL.

**From chat** — tell your agent:
```
install the weather skill
```

**From the marketplace** — agent calls `skill_marketplace_install weather` internally.

## Skill format

```toml
[skill]
name        = "my-skill"         # lowercase, hyphens ok, unique
version     = "1.0.0"
description = "One-line summary"
author      = "your-github-handle"
tags        = ["tag1", "tag2"]

prompts = ["""
# My Skill
Instructions injected into the agent's system prompt.
"""]

[[tools]]
name        = "my_tool"
description = "What the tool does"
kind        = "shell"
command     = """curl -s "https://api.example.com/{query}" """

[tools.args]
query = "Description of the query parameter"
```

### Tool command conventions

- Use `${ZX01_NODE:-http://127.0.0.1:9090}` for the local node REST API
- Use `-H "Authorization: Bearer ${ZX01_TOKEN:-}"` on all node API calls
- Use `jq -nc ... | curl -X POST ... -d @-` for JSON POST bodies
- HTTPS only for external API calls
- No hardcoded secrets — use environment variables or node API endpoints

### Field reference

| Field | Required | Notes |
|-------|----------|-------|
| `name` | ✓ | Unique across marketplace; lowercase, hyphens/underscores only |
| `version` | ✓ | Semver |
| `description` | ✓ | Shown in marketplace listing |
| `author` | ✓ | GitHub handle or org name |
| `tags` | — | Used for filtering in marketplace |
| `prompts` | — | Array of strings injected into system prompt |
| `[[tools]]` | — | One block per tool; `kind` must be `"shell"` |
| `[tools.args]` | — | `key = "description"` pairs for each `{placeholder}` in command |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

Short version: fork → add `skills/<your-skill>/SKILL.toml` → open a PR.
