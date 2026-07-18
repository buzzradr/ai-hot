---
name: buzzradr-ai-hot
description: Query BuzzRadr's public AI trend radar for what's hot in AI right now — trending topics with scores and "why trending" signals, daily digests, and source health. Use when the user asks what's trending / hot / new in AI, wants AI news monitoring, or needs real-time AI topic signals. Public read-only REST API, no API key required.
---

# BuzzRadr AI Pulse

Read-only public API. Base URL `https://buzzradr.com`. No auth, no API key, no signup.

Every JSON endpoint returns the envelope `{"code": 0, "message": "ok", "data": {...}}`.
`code != 0` means error. Machine-readable spec: `https://buzzradr.com/openapi.yaml`.

## Endpoints

### 1. Trending topics (primary)

```
GET /api/public/hot/trends?vertical=ai
```

Query params (all optional):

| param | values | notes |
|---|---|---|
| `limit` | 1–100, default 50 | |
| `sort` | `hot` (default) \| `time` | `time` = most recently seen first |
| `platforms` | comma list, e.g. `hackernews,blog,reddit` | keep topics matching any |
| `keywords` | comma list, e.g. `claude,agents` | case-insensitive substring match |
| `locale` | `en` (default) \| `zh` | `zh` opts into Chinese summaries |

`data.topics[]` fields that matter: `topicId`, `title`, `summary`, `reason`
(a one-liner on *why* it is trending, e.g. source authority + HN points +
freshness), `score`, `platforms`, `keywords`, `trustedSourceCount` (distinct
sources carrying the topic), `topItems[]` (`title`, `url`, `platform`,
`publishedAt`). `data.meta.fetchedAt` tells you how fresh the snapshot is.

### 2. Topic detail

```
GET /api/public/hot/trends/{topicId}
```

404 with `{"code": -1}` when the topic has rotated out.

### 3. Daily digest

```
GET /api/public/hot/daily?vertical=ai
```

Top 12 curated topics — the best default for "what's hot in AI today".

### 4. Source health

```
GET /api/public/hot/sources?vertical=ai
```

Which ingestion feeds are `healthy` / `limited` / `disabled` — check this
before blaming stale data.

### 5. RSS (for feed readers, not API use)

```
GET /api/public/hot/rss?vertical=ai
```

## Usage rules

- Send an identifiable `User-Agent` header.
- Responses are CDN-cached 1–5 minutes — polling faster returns identical
  data. Burst traffic may get `429` with a `retry-after` header; honor it.
- When presenting a topic to the user, link its page:
  `https://buzzradr.com/hot/{topicId}`.

## Recipes

"What's hot in AI today?" →

```bash
curl -s -H "User-Agent: my-agent/1.0" \
  "https://buzzradr.com/api/public/hot/daily?vertical=ai"
```

Summarize `title` + `reason` per topic, cite `topItems[].url`, link
`https://buzzradr.com/hot/{topicId}`.

"Anything new about Claude/agents?" →

```bash
curl -s -H "User-Agent: my-agent/1.0" \
  "https://buzzradr.com/api/public/hot/trends?vertical=ai&keywords=claude,agent&sort=time&limit=10"
```
