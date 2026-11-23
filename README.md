# Google Patents Search MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-google-patents-search/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-google-patents-search) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-google-patents-search%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-google-patents-search&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-google-patents-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-patents-search) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-google-patents-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-patents-search) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-google-patents-search)](https://github.com/mambalabsdev/mcp-google-patents-search/blob/main/LICENSE)

An MCP server for the Mamba Labs Google Patents Search actor on Apify. Google Patents results as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes search terms, an assignee or inventor, filters, or patent numbers and returns one row per patent from Google Patents, with full patent page details when asked.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-google-patents-search": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-google-patents-search"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_google_patents` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find the 20 newest lidar patents assigned to Waymo."
- "What does patent US10000000B2 claim, and is it still active?"

## Tool

`search_google_patents`. One tool. Example arguments:

```json
{
  "queries": [
    "lidar"
  ],
  "assignee": "Waymo",
  "sort": "newest",
  "max_results_per_query": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `queries` | array | no | One search per line, as you would type it on patents.google.com, for example solid state battery. A full patents.google.com search link also works and keeps its filters. Clay users: wrap a column in a one item list. |
| `patent_numbers` | array | no | Optional. Publication numbers with the kind code, for example US10000000B2 or EP3000000A1, or patents.google.com/patent links. Each is read from its patent page and returned with full details. |
| `assignee` | string | no | Optional. Company or owner name, for example Waymo. Applies to every search. With no search terms, it is a search on its own. |
| `inventor` | string | no | Optional. Inventor name. Applies to every search. With no search terms, it is a search on its own. |
| `country` | string | no | Optional. Patent office codes, comma separated, for example US,EP,WO. |
| `status` | string | no | Optional. grant for granted patents only, application for applications only. Empty means both. |
| `patent_type` | string | no | Optional. patent (utility) or design. Empty means both. |
| `language` | string | no | Optional. Document language as Google Patents names it, for example ENGLISH or GERMAN. |
| `after_date` | string | no | Optional. Only patents dated on or after this day, YYYY-MM-DD. The date is the one chosen in Date field. |
| `before_date` | string | no | Optional. Only patents dated on or before this day, YYYY-MM-DD. |
| `date_field` | string | no | Which date After and Before use: priority (default), filing, or publication. |
| `sort` | string | no | Empty for relevance, newest, or oldest. |
| `max_results_per_query` | number | no | Most patents returned per search. Default 20, at most 1000 (Google Patents serves 10 pages of 100). |
| `include_details` | boolean | no | true reads each patent's page too: abstract, claims count and first claim, CPC codes, legal status, expiration, citation counts, and legal events. Charged as a detail row instead of a search row. Default false. |
| `dedupe` | boolean | no | true (default) returns each patent once per run, across all searches. Repeats are not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `patent-row`, one patent from a search | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `patent-detail-row`, one patent with full details | $0.007 | $0.00665 | $0.0063 | $0.00595 |
| `apify-actor-start`, per run start | $0.003 | $0.003 | $0.003 | $0.003 |

Repeats, summaries, pages not found, refused pages, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/google-patents-search](https://apify.com/mambalabs/google-patents-search)

## License

MIT. See [LICENSE](LICENSE).
