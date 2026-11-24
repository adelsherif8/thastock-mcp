# Thastock MCP server

[![Thastock MCP connector](https://glama.ai/mcp/connectors/com.thastock/mcp/badges/score.svg)](https://glama.ai/mcp/connectors/com.thastock/mcp)

An AI second opinion on any US stock, inside ChatGPT, Claude, Cursor or any MCP client.

Ask "should I buy NVDA?" and your assistant gets Thastock's analysis: the price, an AI verdict (bullish, neutral or bearish) with a confidence score, the strongest **bull case**, the strongest **bear case**, and the live numbers behind them.

- **Remote server, nothing to install:** `https://thastock.com/api/mcp` (Streamable HTTP)
- **No sign-in or API key needed**
- **Official MCP Registry:** `com.thastock/mcp`
- **Website:** [thastock.com](https://thastock.com)

## Tool

### `analyze_stock`

| Input | Type | Example |
|---|---|---|
| `symbol` | string, US ticker | `NVDA`, `AAPL`, `BRK.B` |

Returns Markdown with:

- Price and today's move
- AI verdict and confidence (0–100)
- Bull case and bear case, each point tied to a number
- The numbers: RSI (14), 50/200-day moving averages, 52-week position, earnings beat rate and next report date, insider buying or selling (90 days), analyst ratings, news tone, Reddit attention, 12-1 month momentum

Read-only and idempotent. Educational, not financial advice.

## Connect

### Claude (claude.ai and Claude Desktop)
Settings → Connectors → **Add custom connector** → URL `https://thastock.com/api/mcp`

### ChatGPT
Settings → Apps & Connectors → Advanced → enable **Developer mode** → **Create** → URL `https://thastock.com/api/mcp`, authentication: none

### Cursor, Windsurf, VS Code and other clients
Add to your MCP config (see [`examples/mcp.json`](examples/mcp.json)):

```json
{
  "mcpServers": {
    "thastock": { "url": "https://thastock.com/api/mcp" }
  }
}
```

### Clients that only support local (stdio) servers
Use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) as a bridge (see [`examples/claude_desktop_config.json`](examples/claude_desktop_config.json)):

```json
{
  "mcpServers": {
    "thastock": { "command": "npx", "args": ["-y", "mcp-remote", "https://thastock.com/api/mcp"] }
  }
}
```

## Try it with curl

```bash
curl -s -X POST https://thastock.com/api/mcp \
  -H 'content-type: application/json' \
  -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"analyze_stock","arguments":{"symbol":"NVDA"}}}'
```

Example output: [`examples/sample-output.md`](examples/sample-output.md)

## Track record

Every verdict Thastock publishes is logged and scored 30 days later against the real price, misses included: [thastock.com/ai-track-record](https://thastock.com/ai-track-record)

## About this repository

The server is hosted by Thastock at `https://thastock.com/api/mcp`. This repository documents it and holds its [MCP Registry metadata](server.json). Issues and feature requests are welcome here.

## License

Documentation and examples: [MIT](LICENSE).
