# App Store & Google Trends MCP server

A hosted MCP server that gives an AI assistant five read-only tools over public data:

| Tool | What it returns |
|---|---|
| `get_trending_searches` | Google Trends "trending now" searches for a country (title, approximate traffic, start time, linked news) |
| `get_app_store_top_charts` | Apple top free or top paid apps for a country, up to 100 |
| `search_app_store` | App Store search results for a keyword, in the order Apple returns them |
| `get_keyword_rank` | Where a given app (bundle ID or numeric ID) ranks for up to 10 keywords |
| `get_app_details` | Rating, rating count, price, genre, version and release notes for up to 10 apps |

Sources are Apple's public iTunes Search/Lookup API, Apple's public RSS chart feed and the Google Trends RSS feed. No logins, no personal data.

The server runs on Apify. This repository holds only the connection details and the registry metadata; the server code is not here.

Store page and pricing: https://apify.com/webatola/app-store-trends-mcp-server

## Connect

You need a free Apify account. Usage is billed to it per tool call (see the Store page for current prices; free-plan monthly credit covers light use).

### Option A: OAuth through mcp.apify.com

No token in config. Your client opens a browser window to sign in to Apify the first time.

```json
{
  "mcpServers": {
    "app-store-trends": {
      "url": "https://mcp.apify.com/?tools=webatola/app-store-trends-mcp-server"
    }
  }
}
```

Tool names appear with a prefix here, for example `webatola--app-store-trends-mcp-server--get_keyword_rank`.

### Option B: direct endpoint with an API token

Get a token at https://console.apify.com/settings/integrations.

```json
{
  "mcpServers": {
    "app-store-trends": {
      "url": "https://webatola--app-store-trends-mcp-server.apify.actor/mcp",
      "headers": { "Authorization": "Bearer YOUR_APIFY_TOKEN" }
    }
  }
}
```

Claude Code:

```bash
claude mcp add --transport http app-store-trends \
  https://webatola--app-store-trends-mcp-server.apify.actor/mcp \
  --header "Authorization: Bearer YOUR_APIFY_TOKEN"
```

Cursor: put either JSON block into `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project).

## Example prompts

- "Where does app 963034692 rank in the US for habit tracker, streaks and routine?"
- "Show the top 25 paid apps in Germany and group them by genre."
- "What is trending on Google in the UK right now?"

## Limits

- Countries are two-letter codes (`us`, `gb`, `de`). Full names are rejected with an error message.
- Charts: top free and top paid only. Apple's public feed has no top grossing.
- Keyword rank looks at the first ~200 search results. Apple's API ranking is close to, but not always identical with, what a device shows.
- Google Trends RSS returns up to 10 items per country.

## Contact

Made by [Webatola](https://www.webatola.com), an independent software studio.
