# allcams MCP server

Live webcam rooms from five platforms as tools for an AI agent: who is online now, what a category
or a model page says, in 41 languages. One URL, no key, read-only.

> Adult content (18+). The site and every link the server returns are for adults only.

| | |
|---|---|
| Endpoint | `https://allcams.fm/api/mcp` |
| Transport | MCP Streamable HTTP, stateless, JSON responses |
| Auth | none |
| Registry | [`fm.allcams/cams`](https://registry.modelcontextprotocol.io/v0.1/servers/fm.allcams%2Fcams/versions/1.0.0) |
| Docs | <https://allcams.fm/mcp/> (Markdown: <https://allcams.fm/mcp.md>) |
| Agent catalog | <https://allcams.fm/.well-known/ai-catalog.json> |
| llms.txt | <https://allcams.fm/llms.txt> |

## Connect

**Claude Code**

```bash
claude mcp add --transport http allcams https://allcams.fm/api/mcp
```

**Claude.ai / Claude Desktop** — Settings → Connectors → Add custom connector → URL `https://allcams.fm/api/mcp`, no authentication.

**Cursor** (`.cursor/mcp.json`)

```json
{ "mcpServers": { "allcams": { "url": "https://allcams.fm/api/mcp" } } }
```

**Any JSON-RPC client**

```bash
curl -s https://allcams.fm/api/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

**MCP Inspector**

```bash
npx @modelcontextprotocol/inspector --cli https://allcams.fm/api/mcp --method tools/list
```

## Tools

| tool | parameters | returns |
|---|---|---|
| `site_overview` | — | what the site is, key facts, sections, languages (the llms.txt) |
| `list_categories` | `language` (default `en`) | every category of one language with slug and one-line description |
| `get_category` | `category`, `language` | one category page: text, platforms, how many models online, related categories |
| `get_model` | `nickname`, `language` | one model page: online/offline and viewers, platform, languages, statistics, tags, links |
| `list_online` | `category`, `nickname`, `limit`, `language` | models live right now, most viewers first |

Every URL a tool returns is a page on allcams.fm: `page_url` opens the model page, `watch_url` opens the live room.
Give them to users exactly as returned.

Resource: `site://llms.txt`.

## Data

Five platforms (Stripchat, Chaturbate, Cam4, LiveJasmin, BongaCams), one shared vocabulary of categories, 41 languages.
Who is online refreshes every 10 minutes; category and model text is the same the HTML pages show.
Category and model pages also exist as Markdown twins (`/<lang>/<slug>.md`, `/<lang>/model/<nickname>.md`).

## This repository

The server runs inside the allcams.fm backend; this repository holds its public description: `server.json`
(the MCP registry manifest), this README and the licence. Issues and questions: open an issue here.

## Licence

MIT — see [LICENSE](LICENSE).
