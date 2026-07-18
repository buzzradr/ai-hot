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

## Prefer raw access?

- **RSS** — `https://buzzradr.com/api/public/hot/rss?vertical=ai`
- **REST / OpenAPI** — `https://buzzradr.com/openapi.yaml`
- Access guide: https://buzzradr.com/hot/access
