# Firecrawl MCP

**Run it on the Apify Store:** https://apify.com/red.cars/firecrawl-mcp
> [ScraperFleet data page](https://scraperfleet.com/actors/firecrawl-mcp/)
API-based web scraping MCP server for AI agents.

## What is this?

20 tools for web scraping, crawling, search, and data extraction — powered by Firecrawl's API. No proxy rotation. No anti-bot complexity. Just API calls.

## Positioning

**For AI agents that need web data without anti-bot headaches.**

Unlike traditional scraping tools that require proxy rotation, browser stealth, and constant maintenance against anti-bot detection, Firecrawl MCP works through a simple API. The heavy lifting happens server-side.

## Tools

### Scraping
- `scrape_and-extract-from-url` — Scrape a single URL and return structured data
- `batch_scrape-and-extract-from-urls` — Batch scrape multiple URLs

### Crawling
- `crawl_urls` — Crawl a website with configurable depth and scope
- `crawl_get-status` — Get crawl status
- `crawl_cancel` — Cancel a crawl job
- `crawl_errors_get-crawl` — Get crawl errors
- `crawl_get-active` — Get all active crawls

### Search
- `search` — Search for URLs matching a query
- `firecrawl-search_search-and-scrape` — Search and scrape combined

### Extraction
- `extract_data` — Extract structured data from URLs using selectors
- `extract_get-status` — Get extract job status

### Deep Research
- `deep-research_start` — Start deep research job
- `deep-research_get-status` — Get deep research status

### Maps
- `map_urls` — Generate a URL map for a website

### LLM TXT
- `llmstxt_generate-llms-txt` — Generate llms.txt for a site
- `llmstxt_get-llms-txt-status` — Check llms.txt generation status

### Team
- `team_get-credit-usage` — Get team credit usage
- `team_get-token-usage` — Get team token usage

### Utilities
- `context` — Get API domain context
- `sync` — Sync operation
- `export` — Export data
- `import` — Import data
- `sql` — SQL query
- `workflow_status` — Get workflow status
- `workflow_archive` — Archive workflow

## Installation

### Apify

```bash
apify push firecrawl-mcp
```

### Local development

```bash
npm install
npm run build
```

### Environment variables

| Variable | Description |
|----------|-------------|
| `FIRECRAWL_BEARER_AUTH` | Firecrawl API bearer token |

## Usage

### Standalone (batch input)

```json
{
  "tool": "scrape_and-extract-from-url",
  "params": {
    "url": "https://example.com"
  }
}
```

### MCP Protocol (Standby mode)

Send JSON-RPC 2.0 requests to `/mcp`:

```bash
# Initialize
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}'

# List tools
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}'

# Call tool
curl -X POST http://localhost:3000/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"scrape_and-extract-from-url","arguments":{"url":"https://example.com"}}}'
```

## Architecture

```
Apify Actor (handleRequest)
    |
    v
MCPProxy (Node.js child_process)
    |
    v
firecrawl-pp-mcp binary (stdio)
    |
    v
Firecrawl API
```

- Actor spawns `firecrawl-pp-mcp` as a subprocess with stdio transport
- JSON-RPC requests are proxied through stdin/stdout
- PPE charges applied via `Actor.charge()` before tool calls
- Standby HTTP server handles MCP protocol over HTTP

## PPE Pricing

| Tool | Price (USD) |
|------|-------------|
| scrape_and-extract-from-url | $0.10 |
| batch_scrape-and-extract-from-urls | $0.10 |
| crawl_urls | $0.15 |
| map_urls | $0.05 |
| search | $0.08 |
| extract_data | $0.10 |
| deep-research_start | $0.12 |
| firecrawl-search_search-and-scrape | $0.08 |
| context | $0.01 |
| sync | $0.02 |
| export | $0.03 |
| import | $0.03 |
| sql | $0.05 |
| workflow_status | $0.02 |
| workflow_archive | $0.03 |

## Authentication

Firecrawl uses bearer token authentication. Set `FIRECRAWL_BEARER_AUTH` in Apify secrets.

## Key Differentiators

1. **No anti-bot** — API-based scraping means no proxy rotation, no browser fingerprinting, no CAPTCHAs
2. **No API key required** — Bearer token auth via environment variable
3. **20 tools** — Covering scrape, crawl, search, extract, and utilities
4. **PPE ready** — Per-tool pricing via Apify PAY_PER_EVENT model

## GitHub Topics

`firecrawl` `web-scraping` `ai-agents` `no-api-key-required` `mcp` `apify`

## Related

- [Firecrawl Documentation](https://docs.firecrawl.dev)
- [Apify MCP Documentation](https://docs.apify.com/mcp)
- [Printing Press Library](https://github.com/mvanhorn/printing-press-library)

## License

MIT