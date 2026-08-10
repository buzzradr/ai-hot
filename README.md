# @buzzradr/ai-hot

Agent skill for **BuzzRadr AI Pulse** — lets coding agents query BuzzRadr's
public AI trend radar directly: trending topics with "why trending" signals,
daily digests, and source health. Public read-only API, no API key.

## Install

```bash
npx skills add buzzradr/ai-hot
```

Works with Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, OpenCode,
OpenHands, and any agent supported by the [skills CLI](https://github.com/vercel-labs/skills).

## What the agent gets

Once installed, your agent knows how to:

- answer "what's hot in AI today?" from `GET /api/public/hot/daily`
- filter trends by keyword/platform via `GET /api/public/hot/trends`
- link every topic back to `https://buzzradr.com/hot/{topicId}`
- respect the API's CDN-cache and 429 etiquette

See [SKILL.md](./SKILL.md) for the full instructions the agent receives.

## Example prompts

Once the skill is installed, these just work:

- *"What's hot in AI today? Give me the top 5 with why they're trending."*
- *"Anything trending about Claude or agents in the last day?"*
- *"Pull today's AI digest and summarize it in Chinese."*
- *"Which platforms is the OpenAI story spreading on?"*

Every topic links back to a shareable page: `https://buzzradr.com/hot/{topicId}`.

## Sample output

`GET /api/public/hot/trends?limit=1` returns topics shaped like:

```json
{
  "topicId": "ai_topic_2owoz7",
  "title": "Auto mode is now the default in Claude Code",
  "reason": "claude.com · HN 37 pts / 5 comments · 2 sources following up · picked up today",
  "score": 43,
  "platforms": ["hackernews", "blog"],
  "trustedSourceCount": 2,
  "topItems": [{ "title": "...", "url": "...", "platform": "hackernews" }]
}
```

## Prefer MCP?

The same radar is exposed as a **remote MCP server** — see
[buzzradr/mcp](https://github.com/buzzradr/mcp):

```bash
claude mcp add --transport http buzzradr https://buzzradr.com/api/mcp
```

## Prefer raw access?

- **RSS** — `https://buzzradr.com/api/public/hot/rss?vertical=ai`
- **REST / OpenAPI** — `https://buzzradr.com/openapi.yaml`
- Access guide: https://buzzradr.com/hot/access
